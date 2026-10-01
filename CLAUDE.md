# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Classic Tetris in vanilla JavaScript + HTML5 Canvas + CSS. No dependencies, no package.json, no bundler, no transpiler, no test suite, no linter. User-facing text (UI labels, overlay messages, README) is in Spanish — keep it that way.

## Running

Open `index.html` directly, or serve the folder statically (recommended):

    python3 -m http.server 8000   # then open http://localhost:8000
    npx serve .

Verify changes by playing in the browser; there are no automated tests.

## Architecture

Three files: `index.html` (DOM: board canvas, side panel, overlay), `style.css` (dark/arcade theme), `game.js` (all logic, single script, `'use strict'`, no modules).

Key points in `game.js`:

- **Global mutable state**: `board, current, next, score, lines, level, paused, gameOver, lastTime, dropAccum, dropInterval, animId` are module-level `let`s. `init()` resets all of them and is also the restart handler.
- **Cell value = piece type = color index**: `board` is a `ROWS × COLS` matrix of `0` (empty) or `1–8` (7 standard pieces + the "Tuerca" challenge piece, a 3×3 ring with an empty center). `PIECES[i]` shapes store `i` in filled cells, and `COLORS[i]` is its color. Index `0` is `null` in both arrays. Adding a piece means adding to both arrays in the same position; `randomPiece()` derives the piece count from `PIECES.length` (no hardcoded number).
- **Piece lifecycle**: `spawn()` promotes `next` → `current` and triggers game over if the new piece collides at spawn. `lockPiece()` = `merge()` → `clearLines()` → `spawn()`. Gravity (`loop`), `softDrop()` and `hardDrop()` all end in `lockPiece()`.
- **Collision** (`collide(shape, x, y)`) is the single source of truth for movement, rotation (with kick offsets `[0,-1,1,-2,2]` in `tryRotate`), ghost projection (`ghostY`), and spawn-death.
- **Game loop**: `requestAnimationFrame`-based; accumulates `dt` into `dropAccum` and drops one row when it exceeds `dropInterval`. Full redraw every frame (`draw()`: grid → locked board → ghost at alpha 0.2 → current piece). Pause cancels the RAF; unpause resets `lastTime` and calls `loop()` directly.
- **Scoring/levels**: `LINE_SCORES[cleared] * level`; soft drop +1/row, hard drop +2/row. Level = `startLevel + floor(lines/10)` (`startLevel` set by pause-menu select, applied in `init()`); `dropInterval = intervalFor(level)` = `max(100, 1000 - (level-1)*90)` ms. Computed in `clearLines()` and `init()`.
- **HUD** updates only via `updateHUD()` (DOM text). The next-piece preview (`drawNext`) is redrawn only on `spawn()`.
- **Overlay** (`#overlay`) is shared by PAUSE and GAME OVER states; the title/score text is set before `hidden` is removed. PAUSE shows `#pause-menu` (resume, restart, controls, start-level select); GAME OVER shows only `#restart-btn`. `P`/`Escape` toggle pause; game keys ignored while `paused || gameOver`.

## Coupled values

- `COLS × BLOCK` and `ROWS × BLOCK` must match `<canvas id="board" width="300" height="600">` in `index.html`.
- `drawNext` assumes a 4×4 grid of 30px cells, matching `<canvas id="next-canvas" width="120" height="120">`.
- Keybindings are in the `keydown` handler at the bottom of `game.js`. The controls list in `index.html` and the table in `README.md` must stay in sync with that handler.
