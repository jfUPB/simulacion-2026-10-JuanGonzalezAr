# Unidad 2

## Bitácora de proceso de aprendizaje
### Actividad 01🗾
- El que mas me gusto fue el de zach lieberman, ademas de que es un artista el cual ya conocia por vision artificial me parece que es un artista que aborda todo lo que un artista generativo deberia abordar, aletoriedad sin dejar de lado algoritmos, el trabajo que mas me gusto fue land lines, me parece algo muy interesante eso de que a partir de una lineas totalmente aleatorias que uno mismo dibuja salgan lugaraes del mundo que coinciden con esas lineas
### Actividad 02🎱
- Sumar dos vectores consiste en sumar sus componentes x e y, lo que en animación se traduce en actualizar la posición usando la velocidad.
- No funciona porque position y velocity son vectores (objetos), y en JavaScript no se pueden sumar con el operador +; por eso es necesario usar el método .add() de p5.Vector
### Actividad 03🧑‍🚀
- Modifiqué el walker para que su posición se represente mediante un vector en lugar de usar variables separadas para x e y. Esto permite manejar la posición como una sola entidad y prepara el código para trabajar con operaciones vectoriales más complejas. El comportamiento del movimiento sigue siendo un random walk, pero ahora está organizado bajo el modelo de vectores de p5.js.
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
### Actividad 04🥇
- **¿Qué resultado esperas obtener en el programa anterior?** : Por lo que veo del codigo lo que hace es iniciar un vector posicion (x,y) y hace un paso para que esos vectores se conviertan a los valores que le mandamos
- **¿Qué resultado obtuviste?** : Se hizo el paso y el vector posicion que declaramos cambió, ahora la posicion esta en (20,30)
- **¿Qué tipo de paso se está realizando en el código?** : Paso por referencia, no es pasando una copia de ese objeto, sino una referencia al mismo objeto.
- **¿Qué aprendiste?** : Como cambiar la posicion de un vector y como funciona el paso por referencia 

### Actividad 05⏰
- **¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?**: mag() sirve para medir la magnitud de un vector osea entender la distancia desde el origen o la intensidad de un desplazamiento. y magSq() calucla esa magnitud de un vector al cuadrado, una calcula solo la magnitud, magSq() cuando solo necesites comparar magnitudes o cuando la raíz cuadrada no es importante. Es más rápido y eficiente en estos casos y mag() solo para calcular la magnitud y realmente la necesites
- **¿Para qué sirve el método normalize()?**: El método normalize() sirve para ajustar la magnitud de un vector a 1 sin cambiar su dirección.
- **Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?**: Es el producto punto de dos vectores, si quieres saber si dos vectores apuntan en la misma dirección, dirección opuesta, o son perpendiculares, el producto punto te lo dice.
- **El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?**: Instancia:vectorA.dot(vectorB) Usas esta versión cuando ya tienes un vector creado y quieres calcular el producto punto con otro vector.Estática:p5.Vector.dot(vectorA, vectorB)Usas esta versión cuando no tienes un objeto específico y prefieres llamar al método directamente desde la clase p5.Vector.

## Bitácora de aplicación 



## Bitácora de reflexión


