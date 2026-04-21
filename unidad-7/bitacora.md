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
## Bitácora de aplicación 


## Bitácora de reflexión
