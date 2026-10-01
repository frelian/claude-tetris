# Sugerencias para mejorar el Tetris

# Implementa version 1

## 1. Power-ups aleatorios
Cada cierto número de líneas aparece una pieza especial con efecto:
- **Bomba** — destruye un área 3×3
- **Rayo** — limpia fila/columna completa
- **Tinte** — convierte todos los bloques de un color en comodines
- **Gravedad** — compacta huecos del tablero
- **Congelar** — pausa la caída durante 5s

## 2. Piezas nuevas no estándar
Añadir piezas pentominó (de 5 bloques) que aparecen ocasionalmente:
- Pieza `+`
- Pieza `U`
- Pieza `Y`
- Pieza `1×1` (single) como recompensa tras un Tetris
- Pieza `3×3` hueca como reto => HECHA !

## 3. Modo combo y multiplicadores
Sistema de combo encadenado:
- Limpiar líneas en turnos consecutivos multiplica la puntuación (x2, x3, x4...)
- Bonus por **T-spin**
- Bonus por **B2B Tetris**
- Bonus por **Perfect Clear** (dejar el tablero vacío)
- Efectos visuales y sonoros al encadenar

## 4. Modo desafío con objetivos
Niveles con objetivos específicos:
- "Limpia 40 líneas en 2 minutos"
- "Sobrevive con basura subiendo desde abajo cada 10s"
- "Tablero con bloques fijos pre-colocados"
- "Piezas invisibles tras tocar suelo"
- "Rotación inversa en niveles altos"

## 5. Sistema de habilidades cargables
Barra de energía que se llena al limpiar líneas. Al activarla, el jugador elige una habilidad:
- Ver siguientes 5 piezas
- Intercambiar pieza actual por otra del pool
- Ralentizar tiempo 10s
- Deshacer última colocación
- Reservar pieza (hold)


## 6. Sistema de Hold (reservar pieza)
Permitir al jugador guardar la pieza actual en un "bucket" para usarla más tarde:

- **Tecla** — `C` o `Shift` (estándar en Tetris moderno) para enviar la pieza actual al hold
- **Slot de reserva** — panel lateral que muestra la pieza guardada (similar al preview de "next")
- **Intercambio** — si ya hay una pieza en el bucket, al pulsar la tecla se intercambia con la pieza activa
- **Restricción** — solo se puede usar una vez por pieza (bloquear hasta que la pieza actual se asiente), para evitar abusos
- **Indicador visual** — atenuar el slot cuando el hold está bloqueado en el turno actual
- **Estrategia** — útil para reservar la pieza `I` esperando un Tetris, o salvarse de un mal spawn


# Implementa version 2

## Menú de pausa completo

Al pausar (tecla `P` o `Escape`) mostrar un overlay con opciones reales:

- **Reanudar** — vuelve al juego
- **Reiniciar** — nueva partida sin recargar página
- **Ver controles** — lista de teclas dentro del menú
- **Nivel inicial** — selector para elegir con qué nivel empezar la próxima partida
- Bloquear inputs del juego mientras el menú está abierto para evitar movimientos accidentales al volver

---

# Implementa

## Tabla de records local

Guardar las mejores puntuaciones en `localStorage`:

- Top 5 puntuaciones con nombre del jugador (campo de texto al game over)
- Mostrar en pantalla de inicio y en el overlay de game over
- Resaltar si la puntuación actual entra en el top
- Botón para resetear records
- Mostrar también el mejor combo y líneas máximas conseguidas

---

# Implementa

## Temas visuales / skins

Selector de skin que cambia la apariencia completa:

- **Retro** — bloques cuadrados, colores planos (estilo actual)
- **Neon** — fondo negro, glow effect con `shadowBlur` en canvas
- **Pastel** — colores suaves, bordes redondeados simulados
- **Pixel art** — patrón de textura dibujado sobre cada bloque
- Guardar preferencia en `localStorage`; cambio sin recargar aplicando nuevas constantes de color y función de draw