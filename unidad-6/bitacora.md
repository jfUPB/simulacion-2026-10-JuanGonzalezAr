# Unidad 6

## Bitácora de proceso de aprendizaje
## Actividad 01🏧
- **Pieza 1: Fidenza #313 (Estructuras de flujo denso)**
- **Análisis de Decisiones Visuales**:
- **Composición:** Es una composición centrípeta donde las líneas parecen converger y arremolinarse, creando una sensación de profundidad y volumen.
- **Densidad:** Muy alta. Los elementos están tan próximos que la superposición genera texturas complejas, casi como fibras musculares o sedimentos geológicos.
- **Dirección del movimiento:** Sigue curvas sinuosas y cerradas. No hay líneas rectas; todo el movimiento es orgánico y fluido, guiado por un campo de fuerzas invisible.
- **Color:** Utiliza una paleta limitada pero contrastada (negros, blancos y acentos de colores primarios). El color se usa para jerarquizar: los bloques más grandes atraen la mirada primero.
- **Ritmo y Repetición/Variación:** El ritmo es frenético por la cantidad de elementos. La repetición se da en la forma rectangular de los trazos, pero la variación radica en su grosor y curvatura.
---
- **¿Por qué es potente?**
- Es potente porque logra que un algoritmo informático se sienta "físico". La forma en que las curvas evitan colisionar de forma abrupta transmite una armonía natural que recuerda a las corrientes de agua o a la formación de mármol.
---
- **Hipótesis del Sistema:**
- Creo que detrás hay un Flow Field basado en Ruido Perlin, pero con una regla de prevención de colisiones o "espaciado uniforme". Los agentes no solo siguen el flujo, sino que tienen una regla que les impide encimarse demasiado, lo que Hobbs llama "curvas que no se cruzan".
----
- **Pieza 2: Fidenza #658 (Minimalismo y espacio negativo)**
- **Análisis de Decisiones Visuales:**
- **Composición:** Aquí el protagonista es el espacio negativo. La composición es asimétrica y deja "aire" para que las formas respiren.
- **Densidad:** Baja. Hay pocos agentes, lo que permite apreciar la trayectoria individual de cada trazo.
- **Dirección del movimiento:** Las líneas tienen una dirección más lineal y menos turbulenta que en la pieza anterior, transmitiendo calma.
- **Color:** Paletas pasteles y terrosas que refuerzan la sensación de serenidad. El color aquí no compite por atención, sino que acompaña el flujo.
- **Ritmo:** Lento y pausado. La repetición de trazos largos crea una cadencia elegante.
--- 
- **¿Por qué es potente?**
- Su potencia reside en la abstracción. Al haber menos elementos, la mente del espectador completa las trayectorias. Demuestra que en el arte generativo, "menos es más" y que el vacío es una herramienta de diseño tan fuerte como el trazo mismo.
---- 
- **Hipótesis del Sistema:**
- El sistema parece utilizar un campo de flujo con frecuencias de ruido muy bajas (cambios muy suaves en la dirección). Además, sospecho que existe una regla de probabilidad para la escala: el algoritmo decide aleatoriamente que solo unos pocos agentes serán muy largos y anchos, mientras que el resto del lienzo permanece vacío.
----
## Actividad 02↘️
1. ¿**Qué es un agente autónomo?** A diferencia de una partícula simple, un agente tiene un propósito (un objetivo, una amenaza o un camino). Shiffman lo define bajo el modelo de Craig Reynolds como un sistema de tres capas: Acción (el movimiento), Dirección (la toma de decisiones) y Selección de Estrategia (el deseo de alcanzar una meta). En resumen: es un objeto que "quiere" algo.
2. **¿Qué es una steering force?** La steering force (fuerza de dirección) es el mecanismo matemático que permite al agente "maniobrar" de forma fluida. No es una fuerza que golpea al objeto, sino una fuerza de corrección.Se basa en una fórmula sencilla pero poderosa:$$\text{Steering} = \text{Velocidad Deseada} - \text{Velocidad Actual}$$Es la fuerza que el agente aplica para cerrar la brecha entre hacia dónde se mueve ahora y hacia dónde le gustaría moverse para cumplir su objetivo. Es lo que permite que el movimiento no sea rígido, sino que tenga una curvatura orgánica y natural.
3. **Diferencia entre steering force y fuerzas externas (Gravedad, Viento, Fricción)** La diferencia fundamental radica en el origen y la intención:
- Fuerzas Externas (Gravedad, Viento): Son impuestas desde afuera. La partícula no tiene "voz ni voto"; si el viento sopla a la derecha, la partícula es empujada a la derecha. Son fuerzas pasivas y universales.
- Steering Force: Es una fuerza interna y proactiva. Nace del "cerebro" del agente. Mientras que la gravedad siempre empuja hacia abajo, la steering force puede decidir ir hacia arriba si ahí está su objetivo. Es una fuerza limitada por la agilidad del agente (maxforce), lo que le da una personalidad física (un agente puede ser más "nervioso" o más "pesado" para girar que otro).
4. **Utilidad para diseñar comportamiento visual (No solo simular movimiento)** Estas ideas son revolucionarias para el diseño visual porque permiten crear narrativa procedimental:
- Expresividad: Al ajustar la steering force, puedes hacer que un trazo parezca tímido (si huye del mouse suavemente) o agresivo (si lo busca con una fuerza de giro muy alta).
- Composiciones vivas: En lugar de diseñar una imagen estática, diseñas un sistema donde los elementos se organizan solos. Si aplicas estas reglas a miles de agentes (como Tyler Hobbs), obtienes texturas y ritmos que parecen vivos, porque cada línea "decidió" su trayectoria evitando a otras o siguiendo un flujo.
- Interactividad profunda: El usuario ya no solo empuja objetos, sino que se convierte en parte del ecosistema. El agente puede reaccionar a ti de formas complejas: rodeándote, siguiéndote a distancia o imitándote.

## Actividad 03🍎
### 1. Construcción del Sistema
* **¿Cómo está construido el campo de flujo?** Es una **cuadrícula (grid)** 2D que divide el lienzo en celdas. Cada celda funciona como un contenedor de información direccional que afecta a cualquier objeto que pase sobre ella.

* **¿Qué representa cada celda o vector?** Representa un **ángulo o dirección** específica, usualmente calculada con **Ruido Perlin**. Es la "corriente" invisible que dicta hacia dónde debe moverse un agente en ese punto del espacio.



### 2. La Consulta del Agente (Lookup)
* **¿Cómo usa un agente su posición para consultar el campo?** El agente divide su posición $(x, y)$ por la **resolución** de la celda (ej: `col = x / 20`). Así encuentra el índice exacto de la celda donde está ubicado dentro del arreglo del campo.

* **¿Cómo se convierte el vector consultado en una decisión de movimiento?** El vector de la celda se convierte en la **Velocidad Deseada**. El agente aplica la fórmula:  
  `Fuerza = Velocidad Deseada - Velocidad Actual`.  
  Esto genera un giro fluido para alinearse con el flujo.



### 3. Parámetros Críticos

| Parámetro | Impacto Visual |
| :--- | :--- |
| **Resolución** | Celdas grandes = flujos bruscos/rectos. Celdas pequeñas = curvas suaves. |
| **MaxSpeed** | Define el ritmo y la energía del movimiento; afecta el largo de las estelas. |
| **MaxForce** | Define la agilidad. Poca fuerza = giros amplios (inercia). Mucha fuerza = rigidez. |
| **Cantidad** | Define la densidad. Pocos agentes = líneas claras. Muchos = texturas de masa. |

### 4. Experimento y Modificación
* **Modificación:** Reducir la **Escala de Ruido** y bajar la **MaxForce**.  
* **Efecto:** El campo se vuelve casi uniforme (líneas paralelas) y los agentes tardan en reaccionar, creando un movimiento de "barrido" lento y elegante, muy minimalista.

---

### 🧠 Reflexión Creativa
* **Tipo de movimiento:** Coordinado, orgánico y determinista. Se siente como una migración o un fluido natural.
* **Sensaciones:** Armonía, flujo constante y orden invisible.
* **Música ideal:** **Ambient o Minimalismo** (estilo *Max Richter*). Sonidos de evolución lenta que acompañan la repetición visual.

## Actividad 04🐜
### 1. Las Tres Reglas Básicas
El flocking se basa en que cada agente solo mira a sus vecinos cercanos para decidir su movimiento:

* **Separación:** Evitar colisiones. El agente siente una fuerza de repulsión si se acerca demasiado a sus vecinos.
* **Alineación:** Seguir el ritmo. El agente intenta promediar su velocidad y dirección con las de los vecinos que lo rodean.
* **Cohesión:** Mantenerse unidos. El agente siente una atracción hacia el centro de masa (el promedio de las posiciones) de su grupo local.

### 2. Parámetros y Control
El sistema se controla mediante **pesos (weights)** y **radios de visión**:
* **Pesos:** Multiplicadores que definen qué regla es más importante (ej: darle más peso a la separación hace que el grupo sea más esquivo).
* **Radios de percepción:** Determinan qué tan lejos puede "ver" un agente para considerar a alguien como su vecino.
* **MaxSpeed y MaxForce:** Definen la rapidez y la agilidad de los agentes para corregir su rumbo.

### 3. Experimento y Modificación
* **Modificación:** Aumentar drásticamente el peso de **Separación** y reducir el de **Cohesión**, manteniendo una **Alineación** alta.
* **Efecto:** El grupo deja de parecer una bandada compacta para convertirse en una formación de "desfile" o "tráfico". Los agentes se mueven en la misma dirección pero mantienen una distancia social exagerada, creando patrones de líneas paralelas que evitan tocarse.

### 4. Comportamiento Emergente
Dependiendo de los parámetros, el sistema puede ser:
* **Fluido:** Cuando las fuerzas están equilibradas; parece agua o un banco de peces.
* **Nervioso:** Si la `MaxForce` es muy alta y los agentes corrigen su posición constantemente.
* **Estable:** Cuando logran formar grupos que viajan juntos por largo tiempo sin fragmentarse.

### 🧠 Reflexión Creativa

* **¿Qué atmósfera visual produce el flocking?**
    Produce una atmósfera de **inteligencia colectiva y dinamismo**. Se siente como algo vivo y orgánico, donde no hay un líder pero sí un orden. Transmite una sensación de comunidad y protección.

* **¿En qué tipo de relación con una canción funcionaría mejor?**
    Funciona increíble con canciones que tengan **muchas capas o texturas (Polifonía)**. Por ejemplo, en el Jazz o la música clásica donde varios instrumentos hacen cosas distintas pero armonizan en conjunto. También en el **Pop Sintético** con arpegios rápidos; cada nota podría ser un agente que se alinea con el ritmo general de la canción.

## Actividad 05🧟‍♂️
    
### 1. Comparación de Algoritmos como Recursos de Diseño

| Aspecto | Flow Fields (Campos de Flujo) | Flocking (Comportamiento de Bandada) |
| :--- | :--- | :--- |
| **Tipo de movimiento** | Dirigido por el entorno. Fluido, continuo y predeterminado por las "corrientes" del lienzo. | Dirigido por la comunidad. Orgánico, enjambres, cambios bruscos de dirección y agrupación. |
| **Nivel de control visual** | **Alto.** El diseñador esculpe el espacio (ej. usando Ruido Perlin). Los agentes simplemente obedecen ese mapa. | **Bajo/Medio.** El diseñador ajusta los "pesos" (reglas), pero el resultado exacto es impredecible. |
| **Nivel de emergencia** | **Bajo.** La complejidad visual viene de la textura del fondo, no de la interacción entre los agentes (se ignoran entre sí). | **Muy Alto.** La complejidad visual nace exclusivamente de la interacción local entre los agentes. |
| **Atmósfera / Sensación** | Determinismo, inevitabilidad, armonía, viento, agua, tranquilidad o flujo constante. | Vida biológica, inteligencia colectiva, nerviosismo, cardúmenes, bandadas, caos bajo control. |
| **Relación Musical** | Música Ambient, Minimalismo, Drone. Texturas continuas y evolución lenta. | Jazz, IDM, polifonías, crescendos orquestales. Música con múltiples capas interactivas. |
| **Ventajas** | Excelente para composiciones estéticas fijas (Arte Generativo tipo Tyler Hobbs) y texturas de alta densidad. | Excelente para performances en tiempo real (Live Coding / VJing) porque se siente verdaderamente vivo y reacciona rápido. |
| **Limitaciones** | Puede sentirse "robótico" o demasiado ordenado si el ruido no está bien calibrado. Los agentes no tienen "conciencia social". | Es difícil obligarlos a quedarse en una zona específica o formar una composición geométrica exacta. |

---

### 2. Diseño Visual aplicado a Estados de Ánimo Musicales

Si tuviera que diseñar los visuales (VJing) para estas cuatro emociones musicales, esta sería mi elección arquitectónica:

* **Contemplativa $\rightarrow$ Flow Field:**
  * *¿Por qué?* Usaría un campo de flujo con resolución muy suave (Ruido Perlin a gran escala) y agentes muy lentos que dejen una estela larga. La falta de interacción entre los agentes invita a la introspección. El espectador sigue una línea fluida, como un río lento, lo que genera paz mental y foco.

* **Agresiva $\rightarrow$ Flocking:**
  * *¿Por qué?* Llevaría la regla de **Separación** al máximo y la **Cohesión** al mínimo, con una `MaxSpeed` muy alta. Los agentes rechazarían estar cerca unos de otros, creando un comportamiento de evasión violento, paranoico y errático, perfecto para ritmos pesados, distorsión o *breakbeats* rápidos.

* **Melancólica $\rightarrow$ Flow Field:**
  * *¿Por qué?* El Flow Field tiene un tono de inevitabilidad. Configaría los vectores apuntando hacia abajo o en espirales lentas, haciendo que los agentes "caigan" o divaguen sin poder cambiar su destino. Representa la deriva y el aislamiento (como lágrimas en la lluvia), donde cada agente sigue su camino solo y eventualmente se desvanece (bajando su canal Alpha).

* **Eufórica $\rightarrow$ Flocking:**
  * *¿Por qué?* Usaría una **Cohesión** y **Alineación** fortísimas. Miles de agentes uniéndose en un solo ente masivo que viaja a toda velocidad por el lienzo. Este movimiento sincronizado crea una sensación de epicidad y magnitud gigante, ideal para acompañar un *drop* musical de alta energía, donde todas las capas de la canción resuenan en sintonía.


## Bitácora de aplicación 

# 📄 Documento de Diseño: Instrumento Visual Generativo

**Obra:** Marea Magnética (Magnetic Tide)
**Track Musical:** *Ryd* - Steve Lacy

---

### 1. Concepto Visual
La obra es un ecosistema líquido y denso inspirado en un fluido ferromagnético. Busca transmitir la "redondez" y el peso del *groove* característico del Neo-Soul. No se trata de explosiones aleatorias, sino de una masa de agentes visuales que flotan, se contraen y respiran con una elegancia pesada, traduciendo la presión sonora en una marea de partículas controlada por gravedad magnética.

### 2. Relación entre la Visual y la Canción
El sistema funciona como un acompañamiento a la instrumental, ignorando por completo la pista vocal para darle el protagonismo al paisaje sonoro:
* **Frecuencias Bajas (Bajo y Kick):** Representan el cimiento de la canción. Su amplitud controla la fuerza de atracción hacia el centro del lienzo. Cada vez que el bajo entra en su loop, la marea visual se comprime y gana densidad.
* **Frecuencias Medias/Altas (Guitarra y Snare):** Representan la atmósfera. Sus picos de energía inyectan turbulencia al sistema, dispersando la materia en patrones curvos para aliviar la tensión acumulada por el bajo.

### 3. Mapa de Decisiones (Justificación Formal)

| Decisión de Diseño | Intención Narrativa / Conceptual |
| :--- | :--- |
| **Grano Analógico en el lienzo** | Evoca la textura *vintage* y el carácter "lo-fi" de Steve Lacy, dando un peso táctil a la imagen. |
| **Colores Ámbar y Óxido** | Transmiten la calidez de la instrumentación real (guitarras y bajos eléctricos) frente a la estética de los sintetizadores. |
| **Notas Musicales En Rojo** | El cambio morfológico de notas suaves y de color ambar a color rojo intenso rompe la calma visual y genera una tensión o "filo" durante el estribillo. |
| **Fondo Negro Profundo** | Actúa como el silencio musical o el espacio negativo, permitiendo que la vibración del color y la densidad de la masa resalten por contraste. |

### 4. Mapa de Interpretación (Interacción Performativa)
La pieza se ejecuta en vivo. No existen interfaces de usuario (UI); el teclado y el mouse son el instrumento.

| Estado Visual | Interacción (Input) | Comportamiento del Algoritmo |
| :--- | :--- | :--- |
| **1. Groove Base** | **Ninguno** *(Automático)* | **Flocking suave.** Enjambre espeso y cohesivo reaccionando al bajo. El mouse actúa como perturbador que revuelve el líquido. |
| **2. Corrientes** | **Mantener Tecla [A]** | **Flow Field activado.** Se apaga la cohesión. Los agentes siguen trayectorias largas y fluidas por toda la pantalla (acompaña acordes de guitarra). |
| **3. Clímax** | **Toggle Barra [Espacio]** | **Ignición Magnética.** Separación extrema, comportamiento nervioso. El mouse cambia a modo "Creador" estallando nuevas direcciones que hace que las particulas nunca se toquen al hacer clic. |

### 5. Justificación del Algoritmo Elegido
Se optó por una arquitectura híbrida que alterna entre **Flocking** y **Flow Fields**:
* El **Flocking** es indispensable para representar la **colectividad del groove**. Permite simular cómo el bajo de la canción une a todas las partículas en una sola masa rítmica.
* El **Flow Field** es necesario para representar la **atmósfera**. Al cambiar a este algoritmo, los agentes se independizan de la masa central y viajan libremente, traduciendo visualmente la espacialidad y reverberación de la guitarra.

### 6. Explicación de la Relación Audio-Visual
La visualización no es un mero ecualizador gráfico, sino un **ecosistema paramétrico**. Mediante la herramienta `p5.FFT`, se extraen datos en tiempo real de bandas de frecuencia específicas. Este análisis de datos actúa como un conjunto de reglas físicas invisibles: la música dicta *cuánta* gravedad o viento existe en el entorno, pero son los agentes autónomos los que deciden *cómo* reaccionar a esas fuerzas según sus propias propiedades de inercia y velocidad, logrando un comportamiento emergente y orgánico.

### 7. Registro: Uso explícito de IA como materializador

**Autoría Conceptual y Dirección Visual (Santiago):**
El concepto central de **"Marea Magnética"**, la selección del tema musical (*Ryd* de Steve Lacy) y la decisión estética de silenciar visualmente la voz para priorizar la instrumental son de mi total autoría. Yo diseñé la narrativa de la pieza a través de tres estados específicos, seleccioné la paleta de colores basada en la portada de la cancion* y definí el **Mapa de Interpretación Performativa** (asignación de teclas y gestos del mouse). El criterio formal y la intención de la pieza nacieron de mis decisiones como diseñador.

**Apoyo Técnico y Materialización (IA):**
Utilicé la IA como un colaborador técnico y "traductor de código" para los siguientes puntos:
* **Implementación Algorítmica:** Solicité la escritura de las funciones matemáticas base para los comportamientos de *Flocking* y *Flow Fields*, ajustando los pesos de cohesión y separación bajo mis instrucciones.
* **Optimización de Hardware:** La IA fue fundamental para reescribir procesos pesados (como el efecto de grano analógico y el ciclo de revisión de vecinos), permitiendo que el sistema funcionara con fluidez (60 FPS) a pesar de las limitaciones de mi procesador.
* **Iteración y Depuración:** Usé la IA para integrar rápidamente la librería `p5.sound` y para materializar el cambio de morfología de las partículas (de círculos a iconos musicales y distorsiones), asegurando que el código fuera estable durante el cambio de estados.

**Conclusión:**
La IA funcionó como una herramienta de soporte técnico que me permitió superar barreras de programación y optimización. Sin embargo, la **autoría conceptual es humana**: el sistema no es una generación aleatoria, sino la ejecución técnica de un plano de diseño y una narrativa que yo construí previamente.

### 8. Mood Board
- <img width="907" height="907" alt="image" src="https://github.com/user-attachments/assets/145dece1-0bcd-4174-9d8c-f4a3b66f731a" />
- <img width="907" height="907" alt="image" src="https://github.com/user-attachments/assets/361993d9-d5bb-4304-87bd-a587325226a4" />

### 9. Bocetos
- <img width="907" height="907" alt="65a5450a-a364-4e17-8df1-9a94824ab30b" src="https://github.com/user-attachments/assets/119fa42b-97a1-4396-b551-5536ebff1cbe" />
- <img width="907" height="907" alt="9c3d3429-d8c4-4345-8aa8-3670af58a70e" src="https://github.com/user-attachments/assets/b94884a6-7daa-497e-b824-aeae1d57acab" />

### 10. Enlace al Sketch
- [Marea Magnetica](https://editor.p5js.org/JuanGonzalezAr/sketches/mBE4CFGCC)
- [Full Screeh](https://editor.p5js.org/JuanGonzalezAr/full/mBE4CFGCC)
## Bitácora de reflexión
