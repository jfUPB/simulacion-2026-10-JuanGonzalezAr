# Unidad 4

## Bitácora de proceso de aprendizaje
### Actividad 01💢
- Es fascinante cómo un ecosistema visual tan rico nace de la matemática más básica. Usando simples funciones trigonométricas (seno y coseno), Akten crea comportamientos que parecen tener vida propia.
- La obra demuestra el poder de la fase y la frecuencia. Elementos idénticos haciendo exactamente lo mismo, pero con ligeros desfases de tiempo o velocidad, generan una danza hipnótica y coordinada sin necesidad de algoritmos complejos.
### Actividad 02😲
- ¿Qué está pasando y cuál es la interacción?:
-  La simulación muestra un elemento gráfico (una línea con dos círculos) girando en el centro del canvas. La rotación ocurre automáticamente porque la variable global angle aumenta 0.1 en cada ciclo de draw(). Además, hay una interacción simple: si presiono cualquier tecla (keyPressed), el ángulo suma otro 0.1, acelerando el giro
- El misterio del translate y el (0, 0):
- Noté que en el código se usa translate(width / 2, height / 2) y luego los elementos se dibujan en coordenadas relativas al (0, 0), como line(-50, 0, 50, 0). Entendí que esto se hace porque la función rotate() siempre hace girar las cosas alrededor del punto de origen actual. Si no traslado el origen al centro primero, el objeto rotaría anclado a la esquina superior izquierda de la pantalla. Al mover el universo entero al centro, el (0, 0) local queda en medio del canvas.
- La relación entre el sistema de coordenadas y rotate():
- La regla de oro que aprendí hoy: no giro el objeto, giro el papel. rotate(angle) toma la cuadrícula invisible de coordenadas espaciales y la inclina. Por eso, aunque en el código siempre le digo a p5.js que dibuje una línea horizontal plana en el (0, 0), el resultado visual es una línea rotada, porque estoy dibujando sobre un lienzo que ya fue manipulado matemáticamente por el ángulo que va creciendo en cada frame.

#### Analisis simulacion 2
- Identificando el marco Motion 101:
- El motor físico está en el método update() de la clase Mover. Aquí, la aceleración no es arbitraria; se calcula dinámicamente restando la posición del objeto a la posición del mouse (p5.Vector.sub(mouse, this.position)). Esto crea un vector de dirección hacia el cursor. Luego, se aplica el marco clásico: esa dirección se convierte en aceleración, la aceleración se suma a la velocidad, y la velocidad actualiza la posición.
- El poder de heading():
- Para que el rectángulo apunte hacia donde se mueve, el código usa let angle = this.velocity.heading();. Esta función es clave porque toma el vector de velocidad actual y calcula matemáticamente su ángulo de inclinación respecto al eje X horizontal. Me da exactamente el ángulo que necesito para rotar el objeto.
- Aislando el espacio con push() y pop():
- Estas dos funciones son los escudos del sistema de coordenadas. push() guarda el estado actual del universo (el origen sin rotar), me permite trasladar el origen a la posición del objeto (translate(this.position.x, this.position.y)) y rotarlo (rotate(angle)), dibujar el rectángulo, y luego pop() restaura el lienzo a la normalidad. Sin esto, las traslaciones y rotaciones se acumularían en cada frame, creando un caos donde el objeto saldría disparado de la pantalla.
- Centrando el dibujo con rectMode(CENTER):
- Al usar rect(0, 0, 30, 10), normalmente p5.js ancla el rectángulo por su esquina superior izquierda. Cambiar el modo a CENTER asegura que el punto (0, 0) sea el centro geométrico de la figura. Esto es vital para que, al aplicar la rotación, el rectángulo gire sobre su propio eje central y no pivoteando torpemente desde una esquina
## Bitácora de aplicación 



## Bitácora de reflexión

