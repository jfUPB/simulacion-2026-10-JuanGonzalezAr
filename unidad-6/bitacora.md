# Unidad 6

## Bitácora de proceso de aprendizaje
#### Actividad 01🏧
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
### Actividad 02↘️
1. ¿**Qué es un agente autónomo?** A diferencia de una partícula simple, un agente tiene un propósito (un objetivo, una amenaza o un camino). Shiffman lo define bajo el modelo de Craig Reynolds como un sistema de tres capas: Acción (el movimiento), Dirección (la toma de decisiones) y Selección de Estrategia (el deseo de alcanzar una meta). En resumen: es un objeto que "quiere" algo.
2. **¿Qué es una steering force?** La steering force (fuerza de dirección) es el mecanismo matemático que permite al agente "maniobrar" de forma fluida. No es una fuerza que golpea al objeto, sino una fuerza de corrección.Se basa en una fórmula sencilla pero poderosa:$$\text{Steering} = \text{Velocidad Deseada} - \text{Velocidad Actual}$$Es la fuerza que el agente aplica para cerrar la brecha entre hacia dónde se mueve ahora y hacia dónde le gustaría moverse para cumplir su objetivo. Es lo que permite que el movimiento no sea rígido, sino que tenga una curvatura orgánica y natural.
3. **Diferencia entre steering force y fuerzas externas (Gravedad, Viento, Fricción)** La diferencia fundamental radica en el origen y la intención:
- Fuerzas Externas (Gravedad, Viento): Son impuestas desde afuera. La partícula no tiene "voz ni voto"; si el viento sopla a la derecha, la partícula es empujada a la derecha. Son fuerzas pasivas y universales.
- Steering Force: Es una fuerza interna y proactiva. Nace del "cerebro" del agente. Mientras que la gravedad siempre empuja hacia abajo, la steering force puede decidir ir hacia arriba si ahí está su objetivo. Es una fuerza limitada por la agilidad del agente (maxforce), lo que le da una personalidad física (un agente puede ser más "nervioso" o más "pesado" para girar que otro).
4. **Utilidad para diseñar comportamiento visual (No solo simular movimiento)** Estas ideas son revolucionarias para el diseño visual porque permiten crear narrativa procedimental:
- Expresividad: Al ajustar la steering force, puedes hacer que un trazo parezca tímido (si huye del mouse suavemente) o agresivo (si lo busca con una fuerza de giro muy alta).
- Composiciones vivas: En lugar de diseñar una imagen estática, diseñas un sistema donde los elementos se organizan solos. Si aplicas estas reglas a miles de agentes (como Tyler Hobbs), obtienes texturas y ritmos que parecen vivos, porque cada línea "decidió" su trayectoria evitando a otras o siguiendo un flujo.
- Interactividad profunda: El usuario ya no solo empuja objetos, sino que se convierte en parte del ecosistema. El agente puede reaccionar a ti de formas complejas: rodeándote, siguiéndote a distancia o imitándote.

### Actividad 03🍎
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

### Actividad 04🐜
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
    
## Bitácora de aplicación 


## Bitácora de reflexión
