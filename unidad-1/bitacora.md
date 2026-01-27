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

## Bitácora de aplicación 



## Bitácora de reflexión





