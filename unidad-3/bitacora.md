# Unidad 3

## Bitácora de proceso de aprendizaje
### Actividad 01🕋
- Después de escuchar a Robert Hodgin, me invade una sensación abrumadora: siento que, cada vez más, estamos dejando atrofiar partes fundamentales de nuestro cerebro al rendirnos ante la inmediatez de los resultados. El esfuerzo, el proceso, la exploración y hasta la frustración se están diluyendo en un sistema insaciable donde parece importar más "ganar" o terminar rápido que realmente aprender; estamos, literalmente, comiendo sin masticar. Me aterra pensar que nos convertimos lentamente en diletantes condenados a consumir pasivamente lo que el algoritmo nos sirva en bandeja, y me pregunto constantemente si bajo estas reglas del juego aún vale la pena crear.
### Actividad 02🩹
- **Resetear la aceleración:** Usar this.acceleration.mult(0) al final de update() es obligatorio. Borra la pizarra en cada frame y evita que las fuerzas se acumulen descontroladamente.
- **El peligro del paso por referencia:** Los vectores en JavaScript (p5.Vector) comparten la misma memoria. Usar force.div(m) altera y arruina la fuerza original para el resto de los objetos.
- **La solución estática:** Utilizar let f = p5.Vector.div(force, m) genera un vector nuevo. La fuerza original queda intacta.
- **Aplicación práctica:** Esta regla garantiza que las fuerzas interactivas generadas por la detección de movimiento se apliquen de forma consistente a todas las partículas o efectos visuales, sin que el vector se debilite en el proceso.
## Bitácora de aplicación 



## Bitácora de reflexión
