# Unidad 4

## Bitácora de proceso de aprendizaje
### Actividad 01💢
- Es fascinante cómo un ecosistema visual tan rico nace de la matemática más básica. Usando simples funciones trigonométricas (seno y coseno), Akten crea comportamientos que parecen tener vida propia.
- La obra demuestra el poder de la fase y la frecuencia. Elementos idénticos haciendo exactamente lo mismo, pero con ligeros desfases de tiempo o velocidad, generan una danza hipnótica y coordinada sin necesidad de algoritmos complejos.
### Actividad 02😲
- **¿Qué está pasando y cuál es la interacción?:**
-  La simulación muestra un elemento gráfico (una línea con dos círculos) girando en el centro del canvas. La rotación ocurre automáticamente porque la variable global angle aumenta 0.1 en cada ciclo de draw(). Además, hay una interacción simple: si presiono cualquier tecla (keyPressed), el ángulo suma otro 0.1, acelerando el giro
- **El misterio del translate y el (0, 0):**
- Noté que en el código se usa translate(width / 2, height / 2) y luego los elementos se dibujan en coordenadas relativas al (0, 0), como line(-50, 0, 50, 0). Entendí que esto se hace porque la función rotate() siempre hace girar las cosas alrededor del punto de origen actual. Si no traslado el origen al centro primero, el objeto rotaría anclado a la esquina superior izquierda de la pantalla. Al mover el universo entero al centro, el (0, 0) local queda en medio del canvas.
- **La relación entre el sistema de coordenadas y rotate():**
- La regla de oro que aprendí hoy: no giro el objeto, giro el papel. rotate(angle) toma la cuadrícula invisible de coordenadas espaciales y la inclina. Por eso, aunque en el código siempre le digo a p5.js que dibuje una línea horizontal plana en el (0, 0), el resultado visual es una línea rotada, porque estoy dibujando sobre un lienzo que ya fue manipulado matemáticamente por el ángulo que va creciendo en cada frame.

#### Analisis simulacion 2
- **Identificando el marco Motion 101:**
- El motor físico está en el método update() de la clase Mover. Aquí, la aceleración no es arbitraria; se calcula dinámicamente restando la posición del objeto a la posición del mouse (p5.Vector.sub(mouse, this.position)). Esto crea un vector de dirección hacia el cursor. Luego, se aplica el marco clásico: esa dirección se convierte en aceleración, la aceleración se suma a la velocidad, y la velocidad actualiza la posición.
- **El poder de heading():**
- Para que el rectángulo apunte hacia donde se mueve, el código usa let angle = this.velocity.heading();. Esta función es clave porque toma el vector de velocidad actual y calcula matemáticamente su ángulo de inclinación respecto al eje X horizontal. Me da exactamente el ángulo que necesito para rotar el objeto.
- **Aislando el espacio con push() y pop():**
- Estas dos funciones son los escudos del sistema de coordenadas. push() guarda el estado actual del universo (el origen sin rotar), me permite trasladar el origen a la posición del objeto (translate(this.position.x, this.position.y)) y rotarlo (rotate(angle)), dibujar el rectángulo, y luego pop() restaura el lienzo a la normalidad. Sin esto, las traslaciones y rotaciones se acumularían en cada frame, creando un caos donde el objeto saldría disparado de la pantalla.
- **Centrando el dibujo con rectMode(CENTER):**
- Al usar rect(0, 0, 30, 10), normalmente p5.js ancla el rectángulo por su esquina superior izquierda. Cambiar el modo a CENTER asegura que el punto (0, 0) sea el centro geométrico de la figura. Esto es vital para que, al aplicar la rotación, el rectángulo gire sobre su propio eje central y no pivoteando torpemente desde una esquina

### Actividad 03:
- Aplique unas funciones nueva que pueden ayudar a que sea mejor la logica
- keyIsDown(): A diferencia de keyPressed() (que detecta un solo toque), keyIsDown() verifica en cada frame si estás manteniendo presionada la tecla. Esto nos permite aplicar una fuerza de aceleración continua, como pisar el pedal de un carro.
- this.velocity.heading(): Esta es una función mágica de p5.Vector. Calcula automáticamente el ángulo (en radianes) hacia el cual apunta nuestro vector de velocidad.
- Tambien hay una fuerza de friccion que lo que hace es que mientras la tecla no se esta presionando el sistema va frenando el objeto
``` js
let vehicle;

function setup() {
  createCanvas(600, 400);
  vehicle = new Vehicle(width / 2, height / 2);
}

function draw() {
  background(30, 30, 50);

  if (keyIsDown(LEFT_ARROW)) {
    let forceLeft = createVector(-0.2, 0); 
    vehicle.applyForce(forceLeft);
  }
  if (keyIsDown(RIGHT_ARROW)) {
    let forceRight = createVector(0.2, 0);
    vehicle.applyForce(forceRight);
  }

  let friction = vehicle.velocity.copy();
  friction.normalize();
  friction.mult(-0.05); // Coeficiente de fricción suave
  vehicle.applyForce(friction);

  vehicle.update();
  vehicle.checkEdges();
  vehicle.show();
}

class Vehicle {
  constructor(x, y) {
    this.mass = 1;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    // Limitamos la velocidad máxima para que no acelere hasta el infinito
    this.velocity.limit(8); 
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    // Obtenemos el ángulo hacia donde apunta la velocidad
    let angle = this.velocity.heading();

    push(); // Guardamos el estado del canvas
    translate(this.position.x, this.position.y); // Movemos el origen al vehículo
    
    // Si la velocidad es casi cero, evitamos que el triángulo salte a 0 grados bruscamente
    if (this.velocity.mag() > 0.1) {
      rotate(angle);
    }

    fill(255, 150, 50);
    stroke(255);
    strokeWeight(2);
    triangle(20, 0, -15, -10, -15, 10); 
    
    pop(); // Restauramos el estado del canvas
  }

  checkEdges() {
    // Hacemos que si sale por un lado, aparezca por el otro (estilo Pac-Man)
    if (this.position.x > width + 20) this.position.x = -20;
    else if (this.position.x < -20) this.position.x = width + 20;
  }
}
```
## Bitácora de aplicación 



## Bitácora de reflexión





