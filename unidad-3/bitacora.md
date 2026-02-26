# Unidad 3

## Bitácora de proceso de aprendizaje
### Actividad 01🕋
- Después de escuchar a Robert Hodgin, me invade una sensación abrumadora: siento que, cada vez más, estamos dejando atrofiar partes fundamentales de nuestro cerebro al rendirnos ante la inmediatez de los resultados. El esfuerzo, el proceso, la exploración y hasta la frustración se están diluyendo en un sistema insaciable donde parece importar más "ganar" o terminar rápido que realmente aprender; estamos, literalmente, comiendo sin masticar. Me aterra pensar que nos convertimos lentamente en diletantes condenados a consumir pasivamente lo que el algoritmo nos sirva en bandeja, y me pregunto constantemente si bajo estas reglas del juego aún vale la pena crear.
### Actividad 02🩹
- **Resetear la aceleración:** Usar this.acceleration.mult(0) al final de update() es obligatorio. Borra la pizarra en cada frame y evita que las fuerzas se acumulen descontroladamente.
- **El peligro del paso por referencia:** Los vectores en JavaScript (p5.Vector) comparten la misma memoria. Usar force.div(m) altera y arruina la fuerza original para el resto de los objetos.
- **La solución estática:** Utilizar let f = p5.Vector.div(force, m) genera un vector nuevo. La fuerza original queda intacta.
- **Aplicación práctica:** Esta regla garantiza que las fuerzas interactivas generadas por la detección de movimiento se apliquen de forma consistente a todas las partículas o efectos visuales, sin que el vector se debilite en el proceso.

### Actividad 03🥖
- Codigo para la friccion
```js
let mover;

function setup() {
  createCanvas(400, 400);
  mover = new Mover(width / 2, height / 2, 2); // x, y, masa
}

function draw() {
  background(240);

  // 1. Calculamos la fricción
  let friction = mover.velocity.copy();
  friction.normalize();
  friction.mult(-1); // Dirección opuesta
  let mu = 0.05; // Coeficiente de fricción
  friction.mult(mu); // Magnitud
  
  // 2. Aplicamos la fricción
  mover.applyForce(friction);

  mover.update();
  mover.checkEdges();
  mover.show();
}

// Interacción: Haz clic para aplicar una ráfaga de viento aleatoria
function mousePressed() {
  let wind = p5.Vector.random2D();
  wind.mult(5); // Fuerza del empujón
  mover.applyForce(wind);
}


```
- Clase mover
``` js
class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    // ¡Aplicando lo aprendido! Creamos un nuevo vector dividiendo por la masa
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0); // Borrón y cuenta nueva en cada frame
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127);
    circle(this.position.x, this.position.y, this.mass * 16);
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = width;
      this.velocity.x *= -1;
    } else if (this.position.x < 0) {
      this.position.x = 0;
      this.velocity.x *= -1;
    }
    if (this.position.y > height) {
      this.position.y = height;
      this.velocity.y *= -1;
    } else if (this.position.y < 0) {
      this.position.y = 0;
      this.velocity.y *= -1;
    }
  }
}
```
- Resistencia al aire y fluidos
```js
let movers = [];

function setup() {
  createCanvas(400, 400);
  for (let i = 0; i < 5; i++) {
    movers[i] = new Mover(random(width), 0, random(1, 4));
  }
}

function draw() {
  background(255);

  // Dibujamos el "fluido" en la mitad de abajo
  noStroke();
  fill(175, 200, 255, 150);
  rect(0, height / 2, width, height / 2);

  for (let mover of movers) {
    // Gravedad (escalada por masa para que caigan igual en el vacío)
    let gravity = createVector(0, 0.1 * mover.mass);
    mover.applyForce(gravity);

    // Si el mover está en el fluido, aplicamos el Drag
    if (mover.position.y > height / 2) {
      let drag = mover.velocity.copy();
      drag.normalize();
      drag.mult(-1); // Dirección opuesta a la velocidad
      
      let c = 0.05; // Coeficiente de arrastre
      let speedSq = mover.velocity.magSq(); // Velocidad al cuadrado
      drag.mult(c * speedSq); // Magnitud
      
      mover.applyForce(drag);
    }

    mover.update();
    mover.checkEdges();
    mover.show();
  }
}

class Mover {
  constructor(x, y, m) {
    this.mass = m;
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
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127, 200);
    circle(this.position.x, this.position.y, this.mass * 16);
  }

  checkEdges() {
    if (this.position.y > height) {
      this.position.y = height;
      this.velocity.y *= -0.9; // Un poco de pérdida de energía al rebotar
    }
  }
}
```
- Atraccion gravitacional
```js
let mover;
let attractor;

function setup() {
  createCanvas(400, 400);
  mover = new Mover(300, 200, 2);
  // Le damos una velocidad inicial para que orbite y no caiga directo en línea recta
  mover.velocity = createVector(0, 2); 
  attractor = new Attractor();
}

function draw() {
  // Ponemos un fondo con opacidad baja para ver la estela de la órbita
  background(255, 10); 

  let force = attractor.calculateAttraction(mover);
  mover.applyForce(force);
  
  mover.update();
  
  attractor.show();
  mover.show();
}

class Attractor {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.mass = 20;
    this.G = 1; // Constante gravitacional
  }

  calculateAttraction(m) {
    // Dirección de la fuerza
    let force = p5.Vector.sub(this.position, m.position);
    let distance = force.mag();
    
    // Restringimos la distancia para evitar comportamientos extremos al acercarse mucho
    distance = constrain(distance, 5, 25); 
    
    force.normalize();
    
    // Magnitud de la fuerza (F = G * m1 * m2 / d^2)
    let strength = (this.G * this.mass * m.mass) / (distance * distance);
    force.mult(strength);
    return force;
  }

  show() {
    noStroke();
    fill(255, 100, 100);
    circle(this.position.x, this.position.y, this.mass * 2);
  }
}

class Mover {
  constructor(x, y, m) {
    this.mass = m;
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
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    noStroke();
    fill(50);
    circle(this.position.x, this.position.y, this.mass * 8);
  }
}
```
## Bitácora de aplicación 
### Actividad 04⚗️
- **La historia de esta pieza interactiva se titula "La Resiliencia del Enjambre"**. Trata sobre la fragilidad de la conexión (ya sea entre personas, recuerdos o ideas) y cómo los elementos externos pueden perturbarla, pero siempre existe una tendencia natural a volver a unirse.
- En la pantalla, veremos un ecosistema de entidades luminosas. El sistema de reglas y fuerzas que narra esta historia es el siguiente:
- **La Atracción (El vínculo):** Existe un punto central invisible en el lienzo que actúa como un atractor gravitacional. Representa el deseo de pertenencia. Lenta pero inexorablemente, atrae a todas las entidades hacia el centro.
- **El Arrastre (El peso del tiempo/entorno):** Las entidades no se mueven en el vacío. Existe una fuerza de resistencia fluida (drag) en todo el lienzo que frena su aceleración, haciendo que su viaje hacia el centro sea orgánico, pausado y requiera esfuerzo continuo.
- **La Disrupción (Interacción del usuario):** El usuario encarna el caos o la adversidad. Al hacer clic y arrastrar el mouse, el usuario genera un "viento huracanado" repulsivo desde la posición del cursor. Esta fuerza interactiva es mucho más fuerte que la atracción central y dispersa violentamente a las entidades, rompiendo el grupo.
```sketch.js
 let movers = [];
let centerPoint;

function setup() {
  createCanvas(windowWidth, windowHeight);
  for (let i = 0; i < 100; i++) {
    let x = random(width);
    let y = random(height);
    let m = random(0.5, 3);
    movers[i] = new Mover(x, y, m);
  }
  centerPoint = createVector(width / 2, height / 2);
  background(10, 15, 30);
}

function draw() {
  background(10, 15, 30, 40);

  for (let mover of movers) {
    let gravity = p5.Vector.sub(centerPoint, mover.position);
    let distance = gravity.mag();
    distance = constrain(distance, 10, 200); // Evitar fuerzas infinitas
    gravity.normalize();
    let G = 2; 
    let strength = (G * mover.mass) / (distance * distance);
    gravity.mult(strength);
    mover.applyForce(gravity);

    let drag = mover.velocity.copy();
    drag.normalize();
    drag.mult(-1);
    let c = 0.02; 
    let speedSq = mover.velocity.magSq();
    drag.mult(c * speedSq);
    mover.applyForce(drag);

    if (mouseIsPressed) {
      let mousePos = createVector(mouseX, mouseY);
      let repulsion = p5.Vector.sub(mover.position, mousePos);
      let d = repulsion.mag();
      
      if (d < 150) { 
        repulsion.normalize();
        let repulseStrength = map(d, 0, 150, 10, 0); 
        repulsion.mult(repulseStrength);
        mover.applyForce(repulsion);
      }
    }

    mover.update();
    mover.show();
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
  centerPoint = createVector(width / 2, height / 2);
}
```
- mover
```js
class Mover {
  constructor(x, y, m) {
    this.mass = m;
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
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    noStroke();
    fill(255, 200, 100, 150); 
    circle(this.position.x, this.position.y, this.mass * 4);
  }
}
```
- [Enlace al Sketch](https://editor.p5js.org/JuanGonzalezAr/sketches/8lmMqeB7t)
- <img width="939" height="851" alt="image" src="https://github.com/user-attachments/assets/4346c917-ad9a-4127-8661-e2dbfcefac90" />

## Bitácora de reflexión



