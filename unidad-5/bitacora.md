# Unidad 5

## Bitácora de proceso de aprendizaje
### Actividad 01🫀
#### Capa de comportamiento:
- **¿Qué propiedades tiene cada partícula? Clasifícalas: ¿Cuáles definen su estado físico? ¿Cuáles su estado vital?**
- Estado físico: this.position, this.velocity y this.acceleration. Estas definen dónde está y cómo se mueve en el espacio.
- Estado vital: this.lifespan. Esta variable (que empieza en 255.0) es su "reloj de arena" interno.
--- 
- **¿Qué condición determina que una partícula “muere”? ¿Es instantánea o gradual?**
- La muerte la determina la función booleana isDead(), que devuelve verdadero solo cuando this.lifespan < 0.0.
- El evento de la muerte (ser borrada de la memoria) es instantáneo, ocurre en un solo fotograma. Sin embargo, el proceso de morir es gradual; en cada ciclo de la función update(), la partícula pierde energía vital con la resta matemática this.lifespan -= 2.
---
- **¿Cómo se actualiza la partícula en cada frame? Identifica el patrón Motion 101.**
- Dentro de la función update(), encontramos:
- this.velocity.add(this.acceleration); $\rightarrow$ La aceleración (alterada previamente por fuerzas) modifica la velocidad.
- this.position.add(this.velocity); $\rightarrow$ La velocidad modifica la posición actual.
- this.acceleration.mult(0); $\rightarrow$ Se borra la aceleración multiplicándola por cero, dejándola limpia para recalcular las fuerzas en el siguiente fotograma
#### Capa de estructura:
- **¿Quién crea las partículas? ¿En qué momento?**
- Las crea el motor principal de p5.js en la función draw() mediante la instrucción particles.push(new Particle(...)). Al estar dentro de draw(), la creación es un proceso continuo que ocurre en cada fotograma (típicamente 60 veces por segundo), convirtiendo el sistema en un "emisor" constante de partículas.
---
- **¿Quién decide cuándo eliminar una partícula del array?**
- Hay una división de tareas muy elegante aquí (Encapsulamiento). La partícula es la única que sabe si ya está muerta (gracias a su método interno isDead()), pero es el sistema principal (el bucle for en el draw()) el que toma esa información y ejecuta la eliminación real del arreglo usando particles.splice(i, 1).
___
- **¿Por qué se recorre el array en orden inverso para eliminar?**
- Este es un problema clásico de estructuras de datos. Si recorres una lista hacia adelante (0, 1, 2, 3...) y eliminas el elemento en la posición 2, el elemento que estaba en la posición 3 se "rueda" hacia atrás para ocupar el hueco de la posición 2. Pero tu bucle avanza a revisar la posición 3, ¡saltándose por completo el elemento que se acaba de rodar! Al recorrer la lista en reversa (ej. 3, 2, 1, 0), si borras la posición 2, la 3 se rueda, pero no importa porque tu bucle ya evaluó la posición 3 y ahora va hacia la 1. Si no lo haces así, el sistema se saltará partículas vivas y verás parpadeos y errores visuales en pantalla.
___
- **¿Qué pasaría con la memoria y el rendimiento si no las eliminas?**
- Tendrías una "fuga de memoria" (Memory Leak) masiva. Si comentas el splice, estás agregando 60 objetos nuevos por segundo. En un par de minutos tendrías decenas de miles de partículas. Aunque visualmente no las veas (porque su alfa es negativo), el procesador de tu computadora seguiría calculando las matemáticas vectoriales (update) y las llamadas de dibujo (show) de cada una de esas 10,000 partículas en cada fotograma. El Frame Rate (FPS) se desplomaría, la interacción se congelaría y, en una instalación que debe correr por horas, el navegador se quedaría sin memoria RAM y colapsaría.
#### Capa de visualizacion
- **¿Qué elementos visuales usa para representar una partícula?**
- Usa dibujo geométrico en 2D puro: un trazo (stroke(0)), un grosor de línea (strokeWeight(2)), un color de relleno (fill(127)) y la forma geométrica de un círculo (circle(...)).
___
- **¿Cómo se conecta el “tiempo de vida” con la apariencia visual?**
- Este es el truco visual más importante del código. El valor numérico del estado vital (this.lifespan), que va de 255 a 0, se inyecta directamente como el segundo parámetro de las funciones de color: stroke(0, this.lifespan) y fill(127, this.lifespan). En p5.js, ese segundo parámetro es el canal Alfa (transparencia). Esto logra que, a medida que la partícula envejece numéricamente, se desvanezca visualmente, haciendo que su desaparición sea orgánica y suave en lugar de desaparecer de golpe
___
- **Si quisieras cambiar la representación visual, ¿qué cambiarías y qué NO?**
- Lo que SÍ cambiaría: Únicamente el bloque de código dentro del método show(). Podría borrar el circle() y poner un rect(), usar line() conectando la posición anterior con la nueva, o cargar una textura gráfica con image().
- Lo que NO cambiaría: Absolutamente nada de las matemáticas ni de la lógica. No tocar el constructor, ni applyForce(), ni update(), ni isDead(). La física de Motion 101 es completamente agnóstica a la apariencia del objeto; calcular la aceleración de un vector funciona exactamente igual si estás dibujando un píxel, una imagen PNG o un modelo 3D.
### Actividad 02🔃
- **Responsabilidades transferidas de draw() a la clase Emitter**
- En el Ejemplo 4.2, la función global draw() era la responsable de hacer casi todo el trabajo pesado. En este nuevo código, esas responsabilidades se han delegado (encapsulado) dentro de la clase Emitter:
  - **La memoria:** La lista que guarda a los individuos (this.particles = []) ya no es global; ahora le pertenece exclusivamente al emisor.
  - **El nacimiento:** La lógica de crear una nueva partícula (addParticle()) usando coordenadas específicas.
  - **La actualización y limpieza:** El bucle inverso for que ejecuta la física de cada partícula (run()) y revisa su ciclo vital para eliminarla (splice) cuando muere.
___
- **La ventaja de encapsular la lógica de emisión**
- La principal ventaja es la escalabilidad e independencia. Si toda la lógica estuviera suelta en el draw(), solo podrías tener un grupo de partículas en toda la pantalla (como una sola manguera de agua). Al encapsular esta lógica en una clase, conviertes al Emitter en una "fábrica" portátil. Puedes tener 10, 50 o 100 fábricas distintas funcionando al mismo tiempo en diferentes coordenadas de la pantalla, y ninguna interferirá con la otra porque cada una gestiona su propio arreglo privado de partículas
___
- **Creación de Emisores vs. Creación de Partículas**
- **Quién crea los emitters?** El entorno global del programa (el Sketch) guiado por la interacción del usuario. Específicamente, se crean dentro de la función mousePressed(), cada vez que haces clic.
- **¿Quién crea las partículas?** El emisor. El entorno global no sabe nada de partículas; le pide al emisor que trabaje, y es el método interno addParticle() de la clase Emitter el que se encarga de instanciar cada nueva partícula en la coordenada de origen de esa fábrica.
___
- **Diagrama de Jerarquía y Niveles de Colección**
- El sistema ahora tiene dos niveles de colección (un arreglo que contiene objetos, los cuales a su vez contienen sus propios arreglos).
- <img width="1355" height="667" alt="image" src="https://github.com/user-attachments/assets/ff622b9d-b833-4394-a3e7-40a43c9cbfbd" />
___
#### Transferencia conceptual
- **Transferencia conceptual (Sin jerga de programación)**
- En este sistema interactivo, el usuario interviene en el estado del mundo creando un nuevo emisor cada vez que actúa. Este emisor funciona como un gestor autónomo que genera de manera continua nuevas entidades desde un punto de origen fijo. Cada entidad nace con un ciclo de vida determinado y es sometida a una fuerza ambiental constante que altera su comportamiento físico en el espacio. Para asegurar el equilibrio del ecosistema y evitar la saturación, cada emisor administra de forma independiente su propia colección de entidades, evaluando permanentemente su estado vital para descartar a aquellas que han agotado su tiempo.


## Bitácora de aplicación 
### Actividad 05🎨
- **Concepto**: Representaré el ciclo de vida de las estrellas, abarcando desde su formación al agrupar polvo cósmico hasta su violenta muerte en forma de supernova. La idea principal que quiero comunicar es que la destrucción en el universo no es un vacío final, sino un acto vital de fertilización y constante renacimiento

#### Bocetos🥑
- ![WhatsApp Image 2026-03-26 at 11 23 15 AM](https://github.com/user-attachments/assets/faa841e3-2034-4f3c-bf2f-c3a1566026b4)
- ![WhatsApp Image 2026-03-26 at 11 23 15 AM (1)](https://github.com/user-attachments/assets/040a48f7-50ea-4569-a2b2-4504e298ddb7)

#### Mapa de decisiones😲

| Elemento del Sistema | Decisión de Diseño / Significado Narrativo |
| :--- | :--- |
| **Nacimiento (Emisor)** | Las partículas de polvo (`Dust`) se generan espontáneamente al inicio o tras una explosión. Las estrellas (`Star`) **solo** nacen si dos o más partículas de polvo colisionan. **Significado:** La materia prima ya existe en el cosmos. La vida estelar requiere que la materia se acumule y alcance un punto crítico. |
| **Tipo de Partícula 1: Estrella (`Star`)** | Es la clase base. De gran tamaño, pulsa visualmente (usando trigonometría) y su color transiciona de azul (joven/caliente) a rojo (vieja/fría) según avanza su vida. **Significado:** Representa la etapa de madurez y estabilidad. El color y el pulso comunican su desgaste energético a lo largo del tiempo. |
| **Tipo de Partícula 2: Polvo Cósmico (`Dust`)** | Hereda de la clase `Star` (Polimorfismo). Es pequeña, se mueve influenciada por Ruido Perlin y su color se desvanece suavemente. **Significado:** Son los remanentes inestables. Representan la fragilidad; si no logran unirse para formar una estrella, simplemente se disuelven en el olvido del vacío. |
| **Fuerzas (Gravedad y Fricción)** | Se aplica una fricción constante para frenar las partículas. Al hacer clic, se activa un vector de atracción fuerte hacia el cursor. **Significado:** El usuario interactúa inyectando gravedad al sistema. Es la fuerza ordenadora que saca al polvo de su caos para darle un propósito (crear estrellas). |
| **Condición de Muerte (`lifespan`)** | El polvo muere lentamente por desvanecimiento de su canal Alpha. La estrella muere abruptamente tras un límite de tiempo fijo. **Significado:** La muerte térmica (polvo) frente a la muerte catastrófica (estrella). Ambas son inevitables, pero ocurren a ritmos distintos. |
| **Comunicación de la Muerte (Supernova)** | Cuando la condición de muerte de la estrella se cumple, gatilla un método `explode()` antes de ser borrada, emitiendo decenas de nuevas partículas de polvo. **Significado:** La muerte estelar es la semilla de la creación. No es solo "desaparecer", es devolverle la energía al sistema para continuar el ciclo. |
| **Gestión de Memoria (Ley de Conservación)** | Uso estricto de recorridos en reversa (`splice`) y constantes de `MAX_PARTICLES` (300) y `MIN_PARTICLES` (100). **Significado:** A nivel técnico, optimiza el rendimiento y evita que el programa colapse. Narrativamente, representa la Ley de Conservación de la Masa: el universo recicla su materia en un ciclo infinito sin sobrepoblarse ni quedarse vacío. |
| **Interacción del Usuario** | **Mouse Press (Atracción) y Mouse Release (Colapso):** El usuario junta el polvo sosteniendo el clic y, al soltarlo, la materia concentrada colapsa formando estrellas. **Significado:** El espectador deja de ser pasivo y asume el rol de una fuerza cósmica fundamental, siendo el catalizador indispensable para que la experiencia cobre vida. |

#### Capturas:
- <img width="945" height="818" alt="image" src="https://github.com/user-attachments/assets/c1756197-df10-4169-9545-9dd0cd3e66c2" />
- <img width="944" height="845" alt="image" src="https://github.com/user-attachments/assets/0521a4d3-63b0-4791-8779-e59ac0741f1f" />
- <img width="943" height="830" alt="image" src="https://github.com/user-attachments/assets/37debf2e-8742-4427-850e-cb1143f5a4d5" />
#### Sketch:
- [Link Al Sketch](https://editor.p5js.org/JuanGonzalezAr/sketches/kTHNDxwCu)
```js
let particles = [];
const MAX_PARTICLES = 300; 
const MIN_PARTICLES = 100;  

function setup() {
  createCanvas(windowWidth, windowHeight);
  particles = [];
  
  for (let i = 0; i < MIN_PARTICLES; i++) {
    particles.push(new Dust(random(width), random(height)));
  }
}

function draw() {
  background(5, 7, 12, 60);

  
  if (particles.length < MIN_PARTICLES) {
    particles.push(new Dust(random(width), random(height)));
  }

  let gravityWell = createVector(mouseX, mouseY);

  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    
    p.velocity.mult(0.99);

    if (mouseIsPressed) {
      let attraction = p5.Vector.sub(gravityWell, p.position);
      let d = attraction.mag();
      if (d < 300) {
        attraction.normalize();
        attraction.mult(0.5); 
        p.applyForce(attraction);
      }
    }

    p.update();
    p.show();

    if (p.isDead()) {
      if (p instanceof Star) {
        p.explode(); 
      }
      particles.splice(i, 1);
    }
  }
}

class CosmicParticle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D().mult(random(0.5, 2));
    this.acceleration = createVector(0, 0);
    this.mass = 1;
    this.lifespan = 255; 
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 0.5; // El polvo se desvanece poco a poco
  }

  isDead() {
    return this.lifespan < 0;
  }
}

class Star extends CosmicParticle {
  constructor(x, y) {
    super(x, y);
    this.size = random(15, 30);
    this.mass = this.size * 2;
    this.baseLifespan = random(200, 400); 
    this.lifespan = this.baseLifespan;
    this.birthTime = frameCount;
  }

  update() {
    // La estrella vive un tiempo fijo basado en los fotogramas
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan = this.baseLifespan - (frameCount - this.birthTime);
  }

  show() {
    // Oscilación (Latido de la estrella)
    let pulse = sin(frameCount * 0.1) * 3; 
    let currentSize = this.size + pulse;
    
    // Transición de color: Azul (joven) -> Naranja/Rojo (vieja)
    let ageRatio = 1 - (this.lifespan / this.baseLifespan);
    let startColor = color(100, 200, 255); 
    let endColor = color(255, 100, 50);   
    let currentColor = lerpColor(startColor, endColor, ageRatio);
    
    noStroke();
    // Brillo exterior
    for (let i = 0; i < 3; i++) {
      fill(red(currentColor), green(currentColor), blue(currentColor), 40);
      circle(this.position.x, this.position.y, currentSize + i * 8);
    }
    // Núcleo
    fill(255); 
    circle(this.position.x, this.position.y, currentSize * 0.6);
  }

  explode() {
    let particulasAExpulsar = 40;
    for (let i = 0; i < particulasAExpulsar; i++) {
      if (particles.length < MAX_PARTICLES) {
        particles.push(new Dust(this.position.x, this.position.y));
      }
    }
  }

  isDead() {
    return this.lifespan <= 0;
  }
}


class Dust extends CosmicParticle {
  constructor(x, y) {
    super(x, y);
    this.size = random(2, 5);
    this.mass = this.size;
    this.lifespan = random(150, 255); 
    this.color = color(random(100, 255), random(100, 255), random(150, 255));
  }

  update() {
    super.update();
    let n = noise(this.position.x * 0.01, this.position.y * 0.01, frameCount * 0.01);
    let angle = map(n, 0, 1, 0, TWO_PI * 2);
    let drift = p5.Vector.fromAngle(angle).mult(0.05);
    this.applyForce(drift);
  }

  show() {
    noStroke();
    fill(red(this.color), green(this.color), blue(this.color), this.lifespan);
    circle(this.position.x, this.position.y, this.size);
  }
}

function mouseReleased() {
  for (let i = particles.length - 1; i >= 0; i--) {
    let p1 = particles[i];
    if (p1 instanceof Dust) {
      for (let j = i - 1; j >= 0; j--) {
        let p2 = particles[j];
        if (p2 instanceof Dust) {
          let d = dist(p1.position.x, p1.position.y, p2.position.x, p2.position.y);
          if (d < 15) {
            particles.push(new Star(p1.position.x, p1.position.y));
            particles.splice(i, 1);
            particles.splice(j, 1);
            break; 
          }
        }
      }
    }
  }
}
```
## Bitácora de reflexión
