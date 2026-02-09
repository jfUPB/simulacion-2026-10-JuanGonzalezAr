# Unidad 2

## Bitácora de proceso de aprendizaje
### Actividad 01🗾
- El que mas me gusto fue el de zach lieberman, ademas de que es un artista el cual ya conocia por vision artificial me parece que es un artista que aborda todo lo que un artista generativo deberia abordar, aletoriedad sin dejar de lado algoritmos, el trabajo que mas me gusto fue land lines, me parece algo muy interesante eso de que a partir de una lineas totalmente aleatorias que uno mismo dibuja salgan lugaraes del mundo que coinciden con esas lineas
### Actividad 02🎱
- Sumar dos vectores consiste en sumar sus componentes x e y, lo que en animación se traduce en actualizar la posición usando la velocidad.
- No funciona porque position y velocity son vectores (objetos), y en JavaScript no se pueden sumar con el operador +; por eso es necesario usar el método .add() de p5.Vector
### Actividad 03🧑‍🚀
- Modifiqué el walker para que su posición se represente mediante un vector en lugar de usar variables separadas para x e y. Esto permite manejar la posición como una sola entidad y prepara el código para trabajar con operaciones vectoriales más complejas. El comportamiento del movimiento sigue siendo un random walk, pero ahora está organizado bajo el modelo de vectores de p5.js.
```
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
    this.position = createVector(width / 2, height / 2)
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.position);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.position.x++;
    } else if (choice == 1) {
      this.position.x--;
    } else if (choice == 2) {
      this.position.y++;
    } else {
      this.position.y--;
    }
    this.position.x = constrain(this.position.x, 0, width, -1)
    this.position.y = constrain(this.position.y, 0, height, -1)
    
  }
}
```
## Bitácora de aplicación 



## Bitácora de reflexión
