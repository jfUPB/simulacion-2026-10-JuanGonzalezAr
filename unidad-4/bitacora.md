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
### Actividad 04:
- **La modificación para fuerzas acumulativas:**
- La modificación obligatoria es this.acceleration.mult(0); (o matemáticamente lo mismo, this.acceleration = createVector(0,0);), colocada siempre al final del método update().
- Modificacion para el color en el attractor, se hace 
``` js 
// Method to display
  display() {
    ellipseMode(CENTER);
    stroke(0); // Borde negro
    strokeWeight(2); // Un poco más de grosor para que destaque
    
    // CAMBIO DE COLORES
    if (this.dragging) {
      fill('#FF3366'); 
    } else if (this.rollover) {
      fill('#FF9933'); 
    } else {
      fill('#33CC99'); 
    }
    
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }
```
- Para darle vida a los atributos this.dragging y this.rollover del Atractor, comprendí que la clave está en unir el cálculo de distancias geométricas con los eventos nativos de p5.js. La lógica es secuencial: primero, uso la función matemática dist() para medir en cada fotograma la distancia entre la punta del cursor y el centro del atractor; si esa distancia es menor a su radio, significa que lo estoy tocando, activando así el estado de rollover para que cambie de color. Luego, aprovecho funciones como mousePressed y mouseReleased para gestionar el agarre. Si hago clic justo cuando el cursor está sobre el círculo, cambio el estado a dragging, lo que obliga a las coordenadas del atractor a igualarse constantemente con mouseX y mouseY mientras mantengo presionado el botón, arrastrándolo por el lienzo. Al soltar el clic, el estado se desactiva y el atractor se ancla en su nuevo lugar
### Actividad 05:
- El radio $r$ es la hipotenusa (la línea diagonal más larga).
- La coordenada $x$ es el cateto adyacente al ángulo.
- La coordenada $y$ es el cateto opuesto al ángulo.
- Las cordenadas polares sirven para hacer movimientos mas organicos, ya que nuestros equipos leen en (X,Y) entonces con las coordenadas polares. Es como usar una brújula y un radar. Para llegar al mismo punto, el sistema te dice: "Gira hacia el ángulo $\theta$ (theta) y camina en línea recta una distancia $r$ (radio)". Es el lenguaje natural de los círculos, los péndulos y las órbitas
- **Depues de la modificacion:**
- Primero el programa falla porque la función line() intenta usar las variables x y y que ya no existen en el código; y segundo, incluso si lo corrijo usando v.x y v.y, el círculo no traza una órbita, sino que se queda atrapado vibrando en el centro del lienzo.  Esto ocurre por la naturaleza de la función p5.Vector.fromAngle(theta), la cual genera matemáticamente un "vector unitario", es decir, una flecha direccional con una magnitud (longitud) exacta de 1 píxel. Al no aplicarle un radio que estire esa flecha (usando una función como v.mult(r) antes de dibujar), las coordenadas resultantes simplemente oscilan entre -1 y 1, haciendo que el sistema dibuje un círculo perfecto pero de un tamaño microscópico e invisible a simple vista.
- **Depues de la modificacion:**
- Al agregar el segundo parámetro a la función, escribiendo let v = p5.Vector.fromAngle(theta, r), el código vuelve a funcionar perfectamente y el círculo retoma su órbita visible. Esto ocurre porque fromAngle() acepta un argumento opcional de magnitud. Mientras que el primer parámetro (theta) establece la dirección, el segundo (r) define la longitud exacta de la flecha desde el momento en que nace. Con esta simple modificación, p5.js se encarga de resolver internamente toda la trigonometría del seno y el coseno, ahorrándome líneas de código y demostrando la elegancia y eficiencia de usar objetos de tipo Vector para las conversiones entre coordenadas polares y cartesianas.

### Actividad 06:
- En el diseño de sistemas generativos, dominar el seno y el coseno significa descubrir la clave para inyectarle vida orgánica a los píxeles. Ya no se trata de programar movimientos mecánicos y rígidos de un punto a otro, sino de usar estas curvas periódicas para crear coreografías digitales que se sienten naturales y fluidas
```js  
let ampSlider, periodSlider, speedSlider;

function setup() {
  createCanvas(600, 400);

  ampSlider = createSlider(10, 150, 100, 1);
  ampSlider.position(20, 20);

  periodSlider = createSlider(50, 500, 200, 1);
  periodSlider.position(20, 50);

  speedSlider = createSlider(0, 0.2, 0.05, 0.005);
  speedSlider.position(20, 80);
}

function draw() {
  background(30, 30, 50);

  let amplitude = ampSlider.value();
  let period = periodSlider.value();
  let speed = speedSlider.value();

  fill(255);
  noStroke();
  textSize(14);
  textAlign(LEFT, CENTER);
  text("Amplitud (A): " + amplitude, 160, 25);
  text("Periodo (T): " + period + " (Frecuencia: " + (1/period).toFixed(3) + ")", 160, 55);
  text("Velocidad (ωt): " + speed.toFixed(3), 160, 85);

  translate(0, height / 2);
  stroke(100);
  strokeWeight(1);
  line(0, 0, width, 0); 

  noFill();
  stroke(0, 255, 150); 
  strokeWeight(3);
  
  beginShape();
  for (let x = 0; x < width; x += 2) {
    let y = amplitude * sin((TWO_PI * x) / period + (speed * frameCount));
    vertex(x, y);
  }
  endShape();
  
  let xFixed = width / 2;
  let yFixed = amplitude * sin((TWO_PI * xFixed) / period + (speed * frameCount));
  
  // Línea de referencia vertical
  stroke(255, 100);
  strokeWeight(1);
  line(xFixed, -amplitude, xFixed, amplitude);
  
  fill(127);
  noStroke();
  circle(xFixed, yFixed, 40);
  text("Movimiento\nLocal", xFixed + 25, yFixed);
}
```

### Actividad 07:
- Por un lado, utilicé la función noise() (Ruido Perlin) para generar una fuerza de viento. A diferencia de random(), el ruido Perlin me permite simular una fuerza ambiental orgánica y continua, evitando movimientos erráticos y logrando ráfagas suaves.
- Por otro lado, implementé el marco Motion 101 (Fuerza $\rightarrow$ Aceleración $\rightarrow$ Velocidad $\rightarrow$ Posición) en la clase Oscillator. El mayor aprendizaje aquí es cómo separar dos sistemas de movimiento en un mismo objeto: el objeto obedece a las fuerzas ambientales para trasladar su "cuerpo" por todo el lienzo (this.position), y al mismo tiempo, sigue calculando su trigonometría interna (this.angle) para hacer oscilar sus apéndices. El uso estratégico de translate(this.position.x, this.position.y) es el puente que conecta la física del mundo con la trigonometría del objeto.
##### Sketch
```js
let oscillators = [];
let tx = 0; 

function setup() {
  createCanvas(640, 240);
  // Inicializamos los osciladores en posiciones aleatorias del lienzo
  for (let i = 0; i < 10; i++) {
    oscillators.push(new Oscillator(random(width), random(height)));
  }
}

function draw() {
  background(255);


  let windX = map(noise(tx), 0, 1, -0.05, 0.05);
  let wind = createVector(windX, 0);
  tx += 0.01; 

  let gravity = createVector(0, 0.02);

  for (let i = 0; i < oscillators.length; i++) {
    oscillators[i].applyForce(wind);
    
   
    let g = p5.Vector.mult(gravity, oscillators[i].mass);
    oscillators[i].applyForce(g);

    oscillators[i].update();
    oscillators[i].checkEdges(); // Para que no se pierdan en el vacío
    oscillators[i].show();
  }
}
```
##### Oscillator
```js
class Oscillator {
  constructor(x, y) {
    this.mass = random(1, 4);
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);

    this.angle = createVector();
    this.angleVelocity = createVector(random(-0.05, 0.05), random(-0.05, 0.05));
    this.amplitude = createVector(random(20, 40), random(20, 40)); 
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(5); 
    this.position.add(this.velocity);
    this.acceleration.mult(0); 

    this.angle.add(this.angleVelocity);
  }

  show() {
    let oscX = sin(this.angle.x) * this.amplitude.x;
    let oscY = sin(this.angle.y) * this.amplitude.y;

    push();
    translate(this.position.x, this.position.y);

    noStroke();
    fill(100, 150, 255, 150);
    circle(0, 0, this.mass * 12);

    stroke(0);
    strokeWeight(2);
    fill(127);
    line(0, 0, oscX, oscY);
    circle(oscX, oscY, 16);
    pop();
  }

  checkEdges() {
    if (this.position.x > width + 50) this.position.x = -50;
    else if (this.position.x < -50) this.position.x = width + 50;
    
    if (this.position.y > height + 50) this.position.y = -50;
    else if (this.position.y < -50) this.position.y = height + 50;
  }
}
```
### Actividad 08:
- El bucle for ahora dibuja una "fotografía" instantánea de la ola. En un solo frame, calcula dónde deben ir todos los círculos de izquierda a derecha. Para hacer esto, usa una variable temporal llamada angle, a la cual le suma angleVelocity en cada paso.
- El motor del tiempo (startAngle): Si empezáramos a dibujar la ola siempre desde el ángulo 0, veríamos la misma fotografía estática 60 veces por segundo. Al hacer startAngle += 0.05 al final del draw(), le estamos diciendo: "En la próxima fotografía, empieza a dibujar la ola un poquito más adelante". Esa pequeña diferencia continua es lo que tu cerebro percibe como un movimiento fluido y ondulante.
```js
// Variable que controla el "tiempo" o la fase de la ola entera
let startAngle = 0; 
let angleVelocity = 0.2; // Qué tan "apretada" está la ola
let amplitude = 100;     // Qué tan alta es la ola

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);

  stroke(0);
  strokeWeight(2);
  fill(127, 127);

  let angle = startAngle; 

  for (let x = 0; x <= width; x += 24) {
    let y = amplitude * sin(angle);
    circle(x, y + height / 2, 48);
    
    angle += angleVelocity;
  }
  
  startAngle += 0.05; 
}
```
### Actividad 09:
- [Link al codigo con la solucion](https://editor.p5js.org/JuanGonzalezAr/sketches/sdk7Lt4Lo)
- El reto de conectar dos resortes en serie fue puramente físico: aplicar la Tercera Ley de Newton (Acción y Reacción).  Modifiqué la clase Spring para que deje de anclarse a un punto fijo y conecte dos objetos dinámicos (a y b). La clave estuvo en el método connect(): calcular la fuerza elástica para tirar del objeto inferior hacia arriba, e invertir esa misma fuerza (mult(-1)) para tirar del superior hacia abajo. Lo mejor de todo es que la clase Bob original no necesitó ningún cambio; al estar diseñada para acumular fuerzas, procesó este nuevo sistema interconectado a la perfección.
### Actividad 10:
- [Link al sketch](https://editor.p5js.org/JuanGonzalezAr/sketches/s_Bhre-Mn)
- Para conectar dos péndulos en serie, me di cuenta de que no necesitaba alterar la física interna de la clase Pendulum. El truco lógico recae en la cinemática del archivo principal: dentro de la función draw(), forcé a que el vector pivot del segundo péndulo se actualizara constantemente para ser exactamente igual a las coordenadas del bob (la bola) del primer péndulo. De esta forma, mientras el péndulo superior calcula su oscilación anclado al techo, el péndulo inferior calcula la suya colgando dinámicamente del primero, creando la ilusión visual de un sistema interconectado fluido.
## Bitácora de aplicación 



## Bitácora de reflexión













