# Unidad 5

## Bitácora de proceso de aprendizaje
### Actividad 01🫀
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
## Bitácora de aplicación 


## Bitácora de reflexión
