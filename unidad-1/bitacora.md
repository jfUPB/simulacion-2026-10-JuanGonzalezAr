# Unidad 1

## Bitácora de proceso de aprendizaje
### Actividad 01 🧑‍🎨
- La aletoriedad es el nucleo de el arte generativo, lo interesante del arte generativo es esa posibilidad tan inmensa de generar multiples "obras"
### Actividad 02 🚛
#### Modificacion del codigo:
- Espero que la caminata se redireccione mas hacia un lado que otro modificando hacia donde va dirigido antes estaba como centrado y se direcciono hacia la derecha 
``` js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step(20);
  walker.show(20);
}

class Walker {
  constructor() { 
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 2) {
      this.x++;
    } else if (choice == 1) {
      this.y++;
    } else {
      this.y--;
    }
  }
}

```
#### Que Ocurrio?
- La caminata se direcciono mas hacia la derecha 
### Actividad 03 🪚
- Una distribucion uniforme es una distribucion en donde todas las probabilidades tienen el mismo porcentaje de salir, como en el ejemplo de la caminata donde tenemos 4 posibilidades cada una del 25% de salir, y una no uniforme es donde esas posibilidades tienen algunas mas % que otras  
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(10));
    if (choice == 5) {
      this.x++;
    } else if (choice == 6) {
      this.x--;
    } else if (choice ==7) {
      this.y++;
    } else {
      this.y--;
    }
  }
}

```
- Que hice en mi codigo? Simplemente en vez de limitarlo a 4 posibilidades lo extendi a 10 donde cada numero tiene 10% de probabilidad de salir, entonces con mi codigo para que se pudiera inclinar mas hacia la derecha le asigne 5 numeros, osea, 50% de probabilidad y al resto (numero 7 y 8) les doy 10%, y los otros dos numeros que sobran que serian el else tienen un 20% de posibilidad de salir

### Actividad 04 🧑‍⚖️
```js
function setup() {
  createCanvas(640, 240);
  background(200);
}

function draw() {
  //{!1} A normal distribution with mean 320 and standard deviation 60
  
  let x = randomGaussian(320,15) 
  let y = randomGaussian(120, 40)
  noStroke();
  fill(0, 10);
  circle(x, y , 16);
}
```
- <img width="798" height="293" alt="image" src="https://github.com/user-attachments/assets/11b2f6b1-01a6-46fe-9925-0a4c22bbe713" />
- [Link a el sketch en la bitacora](https://editor.p5js.org/JuanGonzalezAr/sketches/4mWWMOm8T)
### Actividad 05🎃
- Usé la técnica de Lévy Flight porque permite simular un movimiento que no es completamente uniforme ni predecible, combinando exploración local con saltos ocasionales de largo alcance. A diferencia de un random walk tradicional, donde todos los pasos tienen un tamaño similar
- El resultado esperado era obtener una trayectoria irregular, con zonas densas donde el walker se mueve lentamente y repite pasos pequeños, interrumpidas por saltos grandes que llevan el punto a regiones nuevas del lienzo
- [Link al sketch](https://editor.p5js.org/JuanGonzalezAr/sketches/XK7F0Re6F)
```js
// The Nature of Code (adaptado a Levy Flight)

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(155);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
  noStroke();
  fill(0, 50);
  circle(this.x, this.y, 5);
}


  step() {
  let r = random(1);

  if (r < 0.01) {
    this.xstep = random(-100, 100);
    this.ystep = random(-100, 100);
  } else {
    this.xstep = random(-1, 1);
    this.ystep = random(-1, 1);
  }

  this.x += this.xstep;
  this.y += this.ystep;

  this.x = constrain(this.x, 0, width);
  this.y = constrain(this.y, 0, height);
}
}
```
- <img width="786" height="298" alt="image" src="https://github.com/user-attachments/assets/9233a121-059c-4a24-b6d9-2aa5028e40d2" />
### Actividad 06 🅰️
- En este cálculo se usa Perlin Noise para determinar el tamaño y la dirección de cada paso del walker. En lugar de generar desplazamientos totalmente aleatorios, se obtiene un valor de ruido que varía de forma continua en el tiempo. Ese valor se transforma con map() a un rango negativo y positivo, permitiendo que el movimiento pueda ir en cualquier dirección, pero siempre de manera suave y coherente con el paso anterior.
- Esperaba un movimiento donde se fuera dibujando una "linea" o dejando un rastro donde visualmente se ve algo muy organico y no solo un movimiento alatorio
- [Link al skecth](https://editor.p5js.org/JuanGonzalezAr/sketches/KlU3g8YYQ)
```js
let x, y;
let tx = 0;
let ty = 150; // distinto para que X y Y no sean iguales

function setup() {
  createCanvas(640, 240);
  background(155);
  x = width / 2;
  y = height / 2;
}

function draw() {
  // Perlin noise devuelve valores suaves entre 0 y 1
  let xstep = map(noise(tx), 0, 1, -2, 2);
  let ystep = map(noise(ty), 0, 1, -2, 2);

  x += xstep;
  y += ystep;

  noStroke();
  fill(0, 60);
  circle(x, y, 5);

  // Avanzamos lentamente en el noise
  tx += 0.01;
  ty += 0.01;

  // Mantener dentro del canvas
  x = constrain(x, 0, width);
  y = constrain(y, 0, height);
}
```


## Bitácora de aplicación 
### Actividad 07🃏
```js
let x, y;
let tx = 0, ty = 1000;
let lastX, lastY;

function setup() {
  createCanvas(800, 450);
  background(0);
  x = width/2; y = height/2;
  lastX = x; lastY = y;
}

function draw() {
  lastX = x; lastY = y;

  let dx = map(noise(tx), 0, 1, -2, 2);
  let dy = map(noise(ty), 0, 1, -2, 2);

  let bigJump = false;
  if (random(1) < 0.001) { 
    dx = random(-180, 180);
    dy = random(-180, 180);
    bigJump = true;
  }

  x += dx; y += dy;

  // tamaño gaussian
  let s = constrain(randomGaussian(4, 1.5), 1, 8);

  // línea suave
  stroke(255, 25);
  line(lastX, lastY, x, y);

  // “estrella” cuando hay salto grande
  noStroke();
  if (bigJump) {
    fill(255, 180);
    circle(x, y, 14);
  } else {
    fill(255, 70);
    circle(x, y, s);
  }

  let speed = map(mouseX, 0, width, 0.002, 0.02);
  tx += speed;
  ty += speed;

  // wrap
  if (x < 0) x = width;
  if (x > width) x = 0;
  if (y < 0) y = height;
  if (y > height) y = 0;
}

function mousePressed() {
  // empuja el ruido para “cambiar” el flujo desde el mouse
  x = mouseX; y = mouseY;
  tx += 1.5;
  ty += 1.5;
}

```
- El concepto de mi sketch está inspirado en la idea de estrellas y constelaciones. A partir de un movimiento continuo se van generando líneas blancas que conectan distintos puntos del lienzo. La mayoría del tiempo el movimiento es suave, pero mediante la técnica de Lévy Flight ocurren saltos grandes ocasionales, los cuales generan puntos más brillantes que representan “estrellas” o destellos. De esta forma, la obra simula la creación de constelaciones de manera generativa y en tiempo real.
- [Link al sketch](https://editor.p5js.org/JuanGonzalezAr/sketches/KOF1XeXqg)
- ![WhatsApp Image 2026-01-27 at 3 18 37 PM](https://github.com/user-attachments/assets/3bbbad3e-f757-4d56-9bc5-c9f3c18d5567)
## Bitácora de reflexión

### Actividad 08🎱
- **Diferencia entre random() y Ruido Perlin (noise()) y cuándo usar cada uno:**
- La aleatoriedad generada por random() produce valores totalmente independientes entre sí, lo que significa que cada resultado no tiene relación con el anterior
- En cambio, el Ruido Perlin (noise()) genera valores que cambian de manera suave y continua en el tiempo. Aunque sigue siendo aleatorio, existe una relación entre un valor y el siguiente
- El random lo usaria cuando quiero hacer cosas que den mas sorpresa o que no sean tan predecibles y el rudo lo usaria para movimiento mas organico dentro de un sketch y dar la sensacion de algo mas natural
- **Qué es una distribución de probabilidad y diferencia visual entre uniforme y normal:**
- Una distribución de probabilidad describe cómo se reparten las posibilidades de que ocurran ciertos valores dentro de un conjunto de datos.
- En una caminata aleatoria con distribución uniforme, todos los valores tienen la misma probabilidad de ocurrir. Visualmente, esto produce movimientos más dispersos y sin concentración clara, ya que cualquier paso es igual de probable.
- En una caminata con distribución normal, la mayoría de los valores se concentran alrededor de un valor central, y los valores extremos ocurren con menor frecuencia. Visualmente, esto genera patrones más naturales
- **Papel de la aleatoriedad en el arte generativo:**
- La aleatoriedad cumple un papel fundamental en el arte generativo porque introduce variación y evita resultados completamente predecibles. Por ejemplo:
  - Generar diversidad visual, haciendo que cada ejecución de la obra sea única
  - Simular comportamientos naturales, ya que muchos fenómenos del mundo real no son totalmente deterministas, sino probabilísticos.
- **Describe uno de los conceptos de aleatoriedad que usaste y explica por qué fue una elección adecuada para lograr el efecto que buscabas:**
- En mi obra utilicé la técnica de Lévy Flight, donde la mayoría de los movimientos son pequeños, pero ocasionalmente ocurren saltos grandes. Esta elección fue adecuada porque permitió simular la aparición de “estrellas” o destellos importantes dentro de la composición
- **¿Qué es un “paseo” o “caminata” (walk) en el contexto de la simulación? ¿Qué característica particular tiene una caminata de tipo “Lévy flight”?:**
- Una caminata o “walk” en el contexto de la simulación es un proceso en el que un punto se mueve paso a paso, donde cada nueva posición depende de la anterior
- La característica principal de una caminata tipo Lévy Flight es que combina muchos pasos pequeños con saltos grandes poco frecuentes. Esta mezcla permite explorar tanto zonas cercanas como regiones lejanas del espacio








