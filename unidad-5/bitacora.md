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
## Bitácora de aplicación 


## Bitácora de reflexión
