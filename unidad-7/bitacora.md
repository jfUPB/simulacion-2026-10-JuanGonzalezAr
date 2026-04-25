# Unidad 7

## Bitácora de proceso de aprendizaje

## Actividad 01:


## 1. Análisis de referentes: Ji Lee
El proyecto de Ji Lee es el estándar de oro en tipografía semántica. Su enfoque se basa en el minimalismo: no añade elementos externos, sino que utiliza la anatomía de las letras para revelar el significado de la palabra.

* **"Oil" (Petróleo):** Lee utiliza la letra "i" para representar una torre de perforación, mientras que el punto de la misma "i" se transforma en una gota cayendo. Es una síntesis visual perfecta donde un carácter alfabético se convierte en un ícono industrial.
* **"Robbery" (Robo):** En este ejemplo, se juega con la posición y la línea base. Una de las letras parece inclinarse o "asaltar" a la otra, o bien, el espacio negativo entre ellas crea una narrativa de sigilo. La tipografía deja de ser estática para sugerir una acción delictiva.
* **"Tunnel" (Túnel):** Utiliza la perspectiva y el escalado de los caracteres para crear un punto de fuga. Las letras parecen alejarse hacia la profundidad de la pantalla, convirtiendo la palabra en el espacio físico que describe.



---

## 2. Propuestas propias: Intención Semántica y Física

Siguiendo los objetivos de la unidad, he propuesto tres conceptos que integran tipografía con el motor de física **Matter.js**:

### A. "BOMB" (Bomba) - [Propuesta Principal]
* **Concepto Visual:** La letra **"O"** se diseña como un círculo perfecto que funciona como el cuerpo de una bomba (con una mecha sutil). El resto de las letras ("B", "M", "B") actúan como fragmentos vinculados.
* **Comportamiento Físico:** Usando Matter.js, la "O" es un cuerpo rígido. Al detectar un pico de audio o un clic, el sistema activa una **fuerza de repulsión radial**, haciendo que las letras salgan disparadas hacia los bordes del lienzo.
* **Relación Audiovisual:** El usuario puede "detonar" la palabra en sincronía con el clímax de una canción.

### B. "WAVE" (Ola)
* **Concepto Visual:** La palabra presenta una línea base irregular. 
* **Comportamiento Físico:** Las letras están conectadas por resortes (*Constraints*) elásticos. El audio de las frecuencias bajas (Bass) genera una ondulación que viaja de izquierda a derecha, haciendo que la palabra fluya como un líquido.

### C. "MELT" (Derretir)
* **Concepto Visual:** Las letras tienen trazos que parecen alargarse.
* **Comportamiento Físico:** Se aplica una gravedad aumentada a través de Matter.js. Las letras son cuerpos deformables que se estiran hacia la parte inferior del canvas según la intensidad del sonido, perdiendo su legibilidad de forma orgánica.

---

## 3. Selección y Justificación: "BOMB"

**¿Por qué me interesa más esta palabra?**
He seleccionado **"BOMB"** porque me permite explorar la **tensión y el impacto** en un entorno audiovisual. A diferencia de un diseño estático, la integración con Matter.js permite que la semántica de la palabra se cumpla a través de una acción física: **la explosión**. 

Técnicamente, me ofrece el reto de manejar fuerzas de repulsión y colisiones, lo cual es ideal para una pieza interpretada en vivo. La "O" deja de ser un carácter para convertirse en el disparador de toda la energía del sistema, creando un vínculo directo entre el diseño tipográfico y la potencia del audio.


# Actividad 2🧟‍♂️

## 1. Definición de Conceptos Clave

Para construir comportamientos físicos en entornos web, es fundamental entender cómo Matter.js organiza su ecosistema. A continuación, defino los pilares del motor con mis propias palabras:

* **Engine (Motor):** Es la unidad central de cálculo. Su función es "resolver" la física en cada cuadro, calculando velocidades, fuerzas y resolviendo colisiones entre objetos basándose en las leyes de Newton.
* **World (Mundo):** Es el contenedor global o "escenario". Representa el espacio donde los objetos existen; si un cuerpo no se añade explícitamente al *World*, el motor no sabrá que debe aplicarle física.
* **Bodies (Cuerpos):** Son los objetos materiales de la simulación. Tienen propiedades físicas como masa, área, fricción y restitución (rebote). Pueden ser cuerpos rígidos (estáticos como un suelo) o dinámicos (que se mueven y caen).
* **Constraint (Restricción):** Es un vínculo que conecta dos cuerpos o un cuerpo con un punto fijo. Se comporta como un resorte o una barra rígida, limitando el movimiento de los objetos para mantenerlos a una distancia específica.
* **MouseConstraint:** Es una herramienta de interacción que vincula el cursor del mouse con los cuerpos del mundo. Permite que el usuario "toque" la física, pudiendo arrastrar o lanzar objetos en tiempo real.



---

## 2. Experimentos de Integración (Matter.js + p5.js)

He realizado dos réplicas experimentales para observar la interacción entre el motor de física y el lienzo de dibujo:

### Experimento A: Gravedad y Colisiones de Cuerpos Rígidos
En este ejercicio, configuré un motor básico donde cajas dinámicas caen sobre una plataforma estática. 
* **Resultado:** Logré mapear las coordenadas de los vértices calculados por Matter.js para dibujarlos usando las funciones de geometría de p5.js (`beginShape` / `vertex`). 
* **Aprendizaje:** Comprendí que p5.js funciona solo como la "capa visual" (render), mientras que Matter.js es el que decide dónde deben estar los puntos en el espacio.
```js
let engine;
let world;
let boxes = [];
let ground;

function setup() {
  createCanvas(windowWidth, windowHeight);
  engine = Matter.Engine.create();
  world = engine.world;
  
  // Suelo estático
  ground = Matter.Bodies.rectangle(width/2, height-20, width, 40, { isStatic: true });
  Matter.World.add(world, ground);
}

function draw() {
  background(15, 18, 15); // Tu color de fondo de bitácora
  Matter.Engine.update(engine);

  if (mouseIsPressed) {
    let b = Matter.Bodies.rectangle(mouseX, mouseY, 40, 40);
    boxes.push(b);
    Matter.World.add(world, b);
  }

  fill(212, 138, 46); // Tu color Ámbar
  for (let box of boxes) {
    beginShape();
    for (let vert of box.vertices) {
      vertex(vert.x, vert.y);
    }
    endShape(CLOSE);
  }
}
```


### Experimento B: Vínculos Elásticos y Suspensión
Este experimento consistió en conectar varios cuerpos mediante *Constraints* para observar el comportamiento elástico.
* **Resultado:** Un sistema donde los objetos reaccionan al movimiento de sus compañeros a través de la tensión de los resortes.
* **Aprendizaje:** Aprendí a manipular la propiedad `stiffness` (rigidez) para generar movimientos orgánicos y fluidos en lugar de conexiones rígidas y mecánicas.

```js
// Alias para facilitar la escritura
const Engine = Matter.Engine;
const World = Matter.World;
const Bodies = Matter.Bodies;
const Constraint = Matter.Constraint;
const MouseConstraint = Matter.MouseConstraint;
const Mouse = Matter.Mouse;

let engine;
let world;
let mConstraint;

let anchor; // Punto fijo en el techo
let box1;   // Letra B
let box2;   // Letra O (Bomba)
let chain;  // El vínculo entre ellas

function setup() {
  createCanvas(windowWidth, windowHeight);

  // 1. Crear el motor y el mundo
  engine = Engine.create();
  world = engine.world;

  // 2. Crear los cuerpos
  // anchor es estático (no se cae), box1 y box2 son dinámicos
  anchor = { x: width / 2, y: 100 };
  box1 = Bodies.rectangle(width / 2, 250, 60, 60);
  box2 = Bodies.rectangle(width / 2 + 100, 250, 80, 80);

  // 3. Crear el Constraint (El resorte entre el techo y la box1)
  let options1 = {
    pointA: anchor,    // Punto en el aire
    bodyB: box1,       // Conectado a la caja 1
    stiffness: 0.1,    // Elasticidad (0 a 1)
    length: 150        // Distancia de la "cuerda"
  };
  let spring1 = Constraint.create(options1);

  // 4. Crear otro Constraint (Uniendo box1 con box2)
  let options2 = {
    bodyA: box1,
    bodyB: box2,
    stiffness: 0.2,
    length: 100
  };
  chain = Constraint.create(options2);

  // 5. Agregar interacción con el mouse
  let canvasMouse = Mouse.create(canvas.elt);
  let mOptions = { mouse: canvasMouse };
  mConstraint = MouseConstraint.create(engine, mOptions);

  // 6. Añadir todo al mundo
  World.add(world, [box1, box2, spring1, chain, mConstraint]);
}

function draw() {
  background(15, 18, 15);
  Engine.update(engine);

  // Dibujar el punto del techo
  fill(255);
  ellipse(anchor.x, anchor.y, 10);

  // Dibujar las conexiones (líneas de los Constraints)
  stroke(212, 138, 46, 100); // Color ámbar transparente
  strokeWeight(2);
  line(anchor.x, anchor.y, box1.position.x, box1.position.y);
  line(box1.position.x, box1.position.y, box2.position.x, box2.position.y);

  // Dibujar los cuerpos (Cajas/Letras)
  noStroke();
  fill(212, 138, 46); // Ámbar de tu paleta
  
  drawBody(box1);
  drawBody(box2);
}

// Función auxiliar para dibujar cualquier cuerpo de Matter.js
function drawBody(body) {
  beginShape();
  for (let v of body.vertices) {
    vertex(v.x, v.y);
  }
  endShape(CLOSE);
}
```
---

## 3. Comportamiento Físico de Interés

Para el desarrollo de mi pieza tipográfica, me interesa explorar la **Transferencia de Energía y Ruptura de Vínculos**. 

Específicamente, busco investigar:
1.  **Tensión elástica:** Cómo mantener una estructura tipográfica legible pero vibrante mediante el uso de *Constraints* que respondan a la amplitud del audio.
2.  **Repulsión Radial:** Cómo aplicar fuerzas de magnitud súbita desde un punto central para generar una expansión violenta de los caracteres.
3.  **Restitución Variable:** Experimentar con el rebote de las letras para que, al chocar con los bordes del lienzo, mantengan su energía kinética y generen una composición visual caótica pero controlada por el ritmo musical.

---




## Bitácora de aplicación 


## Bitácora de reflexión
