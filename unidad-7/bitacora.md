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

# 🔊 Bitácora: Exploración de Audio en p5.js (Actividad 03)

## 1. Introducción
En esta fase, investigué cómo traducir señales sonoras en datos de entrada (*inputs*) capaces de dictar el comportamiento de un motor de física (**Matter.js**). El objetivo principal fue diferenciar entre respuestas **puntuales** (basadas en eventos) y respuestas **continuas** (basadas en estados).

---

## 2. Experimentos Realizados

### Experimento 1: Respuesta Puntual mediante Energía de Bajos (FFT)
Para este ejercicio, utilicé un audio de impacto rítmico (*Dino Stomp*) para observar cómo un pico de energía en frecuencias bajas puede alterar la estabilidad de cuerpos rígidos.

* **Dato leído:** Utilicé la función `fft.getEnergy("bass")`. Este valor analiza el espectro de frecuencias y devuelve la intensidad de los sonidos graves (de 0 a 255).
* **Comportamiento activado:** Implementé un umbral (*threshold*). Cuando la energía de bajos supera el valor de 200 (momento del impacto), el sistema aplica una fuerza física mediante `Matter.Body.applyForce()`.
* **Resultado físico:** Los cuerpos en pantalla reciben un impulso vertical súbito, provocando que "salten" al ritmo exacto de la pisada. Es una respuesta de tipo **puntual**.



### Experimento 2: Respuesta Continua mediante Amplitud (Gravedad Dinámica)
En este segundo ejercicio, utilicé un sonido ambiente de tipo *Ethereal Lush Pad* para generar un comportamiento físico atmosférico y fluido.

* **Dato leído:** Utilicé `p5.Amplitude()`, que entrega un valor normalizado (RMS) entre 0.0 y 1.0 del volumen general de la atmósfera sonora.
* **Comportamiento activado:** Mapeé el valor de la amplitud directamente a la propiedad `engine.world.gravity.y` del motor físico.
* **Resultado físico:** Creé un sistema de "levitación sonora". Dado que el *Lush Pad* tiene crecimientos suaves de volumen, la gravedad del mundo cambia de forma gradual. Cuando el sonido envuelve el espacio, los objetos flotan hacia arriba; cuando el sonido desvanece, recuperan su peso y caen lentamente. Es una respuesta **continua** y orgánica.



---

## 3. Análisis de Datos Sonoros

| Concepto | Aplicación en el Experimento | Utilidad Semántica |
| :--- | :--- | :--- |
| **Amplitud** | Control de gravedad global. | Generar atmósferas y estados de ánimo constantes. |
| **Energía (FFT)** | Detección de golpes de bajo (Bass). | Sincronizar impactos físicos con la percusión. |
| **Respuesta Puntual** | Salto de las cajas en el "stomp". | Ideal para disparar eventos únicos (explosiones). |
| **Respuesta Continua** | Levitación según el "Lush Pad". | Ideal para representar fluidez y movimientos vivos. |

---

## 4. Estrategia Sonora para el Proyecto Final

Para el desarrollo de mi pieza tipográfica, me interesa una combinación de ambas estrategias:

1.  **Respuesta Continua (Amplitud):** Utilizar la amplitud del ambiente para que la palabra vibre o se mantenga en un estado de flotación leve, sugiriendo una "tensión interna" constante.
2.  **Respuesta Puntual (Frecuencias Bajas):** Utilizar picos de energía para disparar la ruptura de los vínculos (*Constraints*) y la explosión definitiva de la palabra.

**Justificación:** Esta dualidad permite que el audio no sea un simple adorno, sino el **motor mecánico** del sistema. El volumen construye la expectativa visual, mientras que el bajo ejecuta la acción semántica de la palabra.

---

# 🖋️ Bitácora: Integración Inicial (Actividad 04)

## 1. Prueba Inicial: "BOMB" - Explosión y Recomposición
Para esta prueba, construí la palabra completa utilizando un sistema de **anclajes elásticos** para cumplir con la intención semántica de una explosión cíclica.

## 2. Desarrollo Técnico
* **Estructura construida:** La palabra "BOMB" completa, donde cada carácter es un cuerpo rígido independiente en Matter.js.
* **Propiedad física manipulada:** Utilicé **Constraints (Restricciones)** vinculadas a puntos fijos en el espacio (`pointA`). Esto crea un comportamiento de "memoria elástica": las letras pueden ser desplazadas violentamente, pero el motor físico siempre buscará devolverlas a su coordenada original.
* **Relación Audio-Física:** Mapeé la energía de los bajos (*Bass*) del audio "Reto Bomb Explotion". Cuando el nivel de energía supera el umbral, se dispara una **fuerza radial** desde la posición de la letra "O", rompiendo la inercia del sistema.

## 3. Evaluación de Significado
* **¿Qué funcionó?** La explosión se siente orgánica y la recomposición de la palabra refuerza la idea de un ciclo. La "O" actúa exitosamente como el detonante visual al cambiar de tamaño y color con el audio.
* **¿Qué no funcionó?** Inicialmente, la gravedad hacía que las letras cayeran al fondo. Tuve que desactivar la gravedad global (`gravity.y = 0`) para que el magnetismo de los anclajes fuera el protagonista y la palabra pudiera formarse nuevamente en el centro de la pantalla.


## Bitácora de aplicación 

# 📝 Bitácora Final: Tipografía Audiovisual Performativa

## 1. Palabra con intención semántica
**Palabra elegida:** BOMB (Bomba).
**Para qué sirve:** La pieza se construye alrededor de los conceptos de tensión acumulada, liberación violenta de energía y daño irreversible. La palabra dicta que el sistema pase de un estado de inmovilidad total a un caos incontrolable, dejando una marca permanente.

## 2. Tipografía semántica
* **Decisión:** Se utilizó la fuente *Arial* en estilo *Bold*, abandonando estéticas elegantes para adoptar un enfoque crudo, brutalista e industrial, similar a una señal de advertencia ("Warning").
* **Semántica:** La letra "O" funciona como el núcleo explosivo. Se diferencia mediante el color rojo puro y un comportamiento orgánico (se hincha), contrastando con la rigidez de las demás letras.

## 3. Comportamiento físico con sentido (Matter.js)
El uso del motor físico no es un adorno, sino la base narrativa de la pieza:
* **Tensión (Constraints):** Resortes invisibles atan las letras a su posición original, simulando la contención de energía antes de estallar.
* **Explosión (Vectores y Velocidad Angular):** Al detonar, se aplican fuerzas radiales masivas y rotación aleatoria, simulando una onda expansiva que saca los cuerpos del lienzo.
* **Daño permanente (Alteración de anclajes):** Durante la explosión, las coordenadas de retorno (`pointA`) se alteran matemáticamente. Cuando las letras son atraídas de vuelta, la palabra se forma "chueca" o desalineada, representando la secuela y el desorden post-traumático de una explosión real.

## 4. Respuesta al audio con sentido semántico (p5.sound)
El audio (`p5.FFT`) es el motor de los estados de la materia:
* **Ignición:** El análisis de las frecuencias bajas (Bass) controla directamente el diámetro de la "O", haciéndola "latir" o temblar al ritmo del sonido.
* **Detonación:** Un umbral específico de energía en los bajos (`bass > 235`) actúa como el interruptor lógico (`if`) que rompe el sistema y desata la explosión, logrando una sincronía audiovisual perfecta.

## 6. Interacción performativa
La obra fue diseñada como un instrumento audiovisual de ejecución en vivo.
* **Ejecución:** El intérprete "prepara" la escena asegurando la inmovilidad y el silencio en pantalla gigante. Al presionar la Barra Espaciadora en el momento dramático adecuado, se enciende la mecha y comienza la tensión.
* El intérprete no guía la explosión; permite que el sistema algorítmico y físico reaccione al audio de forma autónoma, convirtiéndose en espectador del caos junto con la audiencia.

## 7. Moodboard y Referencias
* **Estética Visual:** Brutalismo tipográfico, diseño industrial, paleta de colores de peligro (Rojo, Naranja, Gris Asfalto).
* **Atmósfera:** La anticipación visual en películas de suspense y la estética *Grime* o destructiva del arte generativo.
* **Referencias:** Explosiones controladas y marcas de pólvora en el arte de Cai Guo-Qiang; comportamientos tipográficos elásticos de Zach Lieberman.

## 8. Bocetos
*(Nota: Aquí debes insertar fotografías de los dibujos a mano alzada que hiciste en papel mostrando la palabra unida, la explosión y el resultado deformado).*

## 9. Mapa de Decisiones
| Elemento | Decisión Técnica | Significado / Intención |
| :--- | :--- | :--- |
| **Forma** | Tipografía pesada (Arial Bold). | Transmite peso, peligro y estética industrial. |
| **Física** | Fricción de aire mínima (`frictionAir: 0.005`). | Permite una onda expansiva masiva que sale del lienzo. |
| **Física** | Alteración matemática del `pointA`. | Representa la cicatriz y la incapacidad de volver a la normalidad tras la explosión. |
| **Audio** | Mapeo de volumen (FFT) a la escala de la 'O'. | Visualiza la acumulación de presión y temperatura inestable. |
| **Visual** | Capa secundaria (`p5.Graphics`) con partículas oscuras fijas. | Representa el hollín y la huella térmica que deja la detonación en el entorno físico. |

## 10. Mapa de Interpretación (Ejecución en Vivo)
1. **Setup:** Carga del sistema. Ajuste a pantalla completa (F) y actualización (F5) para centrar la masa de letras en un lienzo oscuro.
2. **El Silencio:** Interacción cero. Se genera expectativa en el público.
3. **La Ignición:** Interacción con la Barra Espaciadora. Inicia el audio, la mecha aparece consumiéndose, y el sistema entra en estado crítico (latido rojo).
4. **El Clímax:** Sin intervención humana, el audio alcanza el umbral de energía, detona el *flash* visual, rompe las inercias y mancha el fondo.
5. **La Consecuencia:** El intérprete deja que las letras regresen exhaustas y desordenadas. Fin de la performance.

## 11. Uso explícito de IA como materializador
En este proyecto, la Inteligencia Artificial (Gemini) se utilizó como un asistente de materialización técnica, manteniendo una clara distinción entre la autoría conceptual y el soporte de código:
* **Mi autoría (Decisiones conceptuales y de diseño):** Fui el director creativo del proyecto. Elegí la palabra "BOMB", definí la estética brutalista y diseñé el flujo narrativo (la mecha, el latido de la "O", el desorden post-explosión y las manchas permanentes). También tomé las decisiones sobre cómo debía ser la coreografía de interacción para la presentación final.
* **Soporte IA (Resolución de sintaxis y matemática):** La IA se encargó de traducir mis ideas artísticas al lenguaje de programación de `p5.js` y `Matter.js`. Me asistió escribiendo los algoritmos para los *Constraints* elásticos, optimizando el rendimiento usando *p5.Graphics* para los residuos fijos, configurando la función `windowResized()` para evitar la desalineación geométrica, y calibrando los multiplicadores de fuerza física para lograr el impacto visual que yo deseaba.

## 12. Codigo fuente y enlace al sketch
- [Sketch](https://editor.p5js.org/JuanGonzalezAr/sketches/bCZoMl5FP)
## Bitácora de reflexión
