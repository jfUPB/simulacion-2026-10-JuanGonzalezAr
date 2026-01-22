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

## Bitácora de aplicación 



## Bitácora de reflexión

