# RM Song Generator: Sistema de Composición Musical Algorítmica

**RM Song Generator** es un sistema algorítmico modular para la composición musical. Utiliza teoría musical avanzada, combinatoria, lógica simbólica y principios de ciencia de datos para generar piezas únicas, estructuradas y reproducibles. A diferencia de los modelos basados en redes neuronales, se basa en una arquitectura determinista basada en reglas, con segmentación explícita y un sistema jerárquico de decisiones.

🔧 **Arquitectura Central**

RM Song Generator no utiliza plantillas ni aprendizaje supervisado. En su lugar, se construye a partir de:

- Una base de datos de más de 1.000 escalas y modos, incluyendo sus grados armónicos.
- Un motor de análisis armónico que realiza más de 1 millón de comparaciones precalculadas entre escalas para determinar modulaciones compatibles, incluyendo acordes de transición y puntuaciones de similitud.
- Un módulo de generación rítmica que selecciona compases y tempo por sección, y genera duraciones internas usando subdivisiones combinatorias (corcheas, tresillos, sextillos, etc.).
- Un motor de selección melódica que elige probabilísticamente entre cuatro fuentes escalares por acorde: modo actual, modo relativo, escala extranjera o combinación híbrida. Produce planes melódicos estructurados: díadas, tríadas, pentatónicas, fugas y más.
- Un motor de arpegios y solos capaz de generar acompañamientos dinámicos y líneas solistas basadas en patrones paramétricos, dirección, lógica de repetición y expansión de grados.
- Un sistema de memoria armónica que almacena secciones generadas previamente y las reintroduce mediante modulaciones justificadas utilizando escalas puente desde un grafo de compatibilidad modal.

Cada sección se construye de manera independiente con su propio tempo, compás, contenido armónico y lógica melódica. La salida final se renderiza en `.mp3` utilizando siete instrumentos fijos: contrabajo, violonchelo, viola, violín grave/agudo, clarinete y arpa.

🌀 **Flujo de Generación**

1. **Ritmo Inicial y Reconocimiento Modal**

- Comienza con la selección aleatoria de compás (por ejemplo, 3/4, 5/4, 6/8) y tempo (20–300 BPM).
- Genera una secuencia pseudorandom de acordes (de 2 a 4 notas) con duraciones que suman el total de la sección.
- Identifica un acorde de referencia probable (usualmente el más largo) e infiere el modo y la tónica mediante coincidencia ponderada de intervalos contra todos los modos conocidos.

  - Coincidencia de raíz = peso completo  
  - 3ra, 5ta, 7ma, 9na, 11va, 13ra = pesos decrecientes  

- Se selecciona como base armónica el modo con mayor ajuste que supere un umbral.

2. **Reordenamiento Tonal y Estructura de Progresiones**

- Una vez establecida la tónica y el modo, los acordes se reordenan para lograr la mayor continuidad armónica.
  - Los acordes previos a la tónica se ordenan por similitud directa (forward).
  - Los acordes posteriores por similitud inversa (reverse).
- Se utiliza opcionalmente una biblioteca de progresiones jazzísticas (200+ recursos, como II–V–I o III–VI–II–V) para validar el flujo armónico.  
  Si no encaja ninguna o si la proyección idiomática está desactivada (por válvula aleatoria), se genera una modulación libre.

A lo largo del proceso, el acorde tónica tiende a actuar como ancla armónica: busca protagonismo estructural alineándose con momentos de reposo o estabilización dentro de la pieza, ayudando a organizar la composición incluso en secciones no idiomáticas o exploratorias.

3. **Arpegios y Expansión Armónica**

Algunos acordes se transforman en arpegios.  
Esto implica:

- Subagrupación de notas (1–3–5, u otras)
- Aplicación de estructuras espejadas, en bucle o expandidas
- Adición de notas de color (7ma a 13ra)

Las duraciones rítmicas se asignan proporcionalmente usando valores de nota, grupos irregulares (como tresillos o quintillos), y ajustes finales para encajar con precisión dentro del compás.  
Aunque el sistema no implementa swing interpretativo, genera líneas rítmicamente variadas y musicalmente coherentes mediante una colocación probabilística de duraciones.

4. **Diseño Melódico y Solos**

La generación melódica se adapta al contexto: modo, tónica, tempo, compás y progresión.  
La elección de escala es probabilística:

- Modo actual
- Modo relativo
- Escala externa

Las frases melódicas utilizan generadores estructurados: díadas, tríadas, fugas, ciclos o pentatónicas.  
Soporta variación rítmica, silencios y desarrollo de motivos.

El clarinete es la voz melódica por defecto. El arpa puede tomar el protagonismo para generar contraste.

5. **Solos de Arpa y Estructuras de Fuga**

Generados como estructuras expresivas y recursivas basadas en grados y ritmo:

- Arpegios expandidos  
- Fugas de 2 notas  
- Alternancia de motivos (por ejemplo, 1–5–3 alternando con tonos más agudos)  
- Respuestas descendentes tras movimientos ascendentes

Los patrones rítmicos pueden incluir grupos irregulares, agrupaciones inusuales y motivos sincopados, pero actualmente **no** se admite superposición polirrítmica completa ni distribución expresiva de silencios como parte de la dinámica interpretativa.

6. **Memoria y Modulación Entre Secciones**

Se almacenan secciones completas (acordes, melodías, tempo, modo, tónica).

La reutilización se gestiona mediante:

- Evaluación de distancia modal (a partir del grafo de compatibilidad)  
- Detección de escalas puente  
- Generación de un pasaje intermedio completo  
- Reexposición de una sección anterior

7. **Orquestación y Renderizado de Audio**

Cada instrumento recibe notas basadas en restricciones de registro y reglas de conducción de voces.  
Los arpegios se distribuyen entre los instrumentos de cuerda; el arpa interpreta líneas rápidas y virtuosas.

La expresividad se codifica mediante:

- CC11 (expresión),  
- Velocidad (velocity),  
- Curvas dinámicas.

Los eventos MIDI se renderizan con **FluidSynth** y soundfonts personalizados, y se exportan a `.mp3`.

🎯 **Aportes y Posicionamiento**

RM Song Generator ofrece una alternativa transparente a las redes neuronales:

| Característica               | Redes Neuronales         | RM Song Generator           |
|-----------------------------|--------------------------|-----------------------------|
| Transparencia               | Caja negra               | Totalmente explicable       |
| Reproducibilidad            | No garantizada           | Garantizada                 |
| Control                     | Limitado                 | Control total de parámetros |
| Profundidad armónica        | Aprendida estadísticamente | Basada en teoría formal     |
| Manejo de modulación        | Implícito o ausente      | Explícito con escalas puente|
| Adaptabilidad teórica       | Requiere reentrenamiento | Extensible vía lógica modal |

RM combina rigor algorítmico y conciencia musical, contribuyendo a:

- Lógica tonal determinista con validación progresiva  
- Más de 1 millón de comparaciones precalculadas entre escalas para modulación  
- Hibridación de progresiones idiomáticas y generación libre  
- Arquitectura modular, trazable y extensible  
- Coherencia formal mediante memoria y reintroducción de secciones  
- Salida reproducible en `.mp3` con orquestación expresiva

🔬 **Aplicaciones y Direcciones Futuras**

🎨 **Uso Creativo**
- Composición (cine, videojuegos, teatro)
- Exploración sonora experimental
- Motor de acompañamiento

📚 **Uso Educativo**
- Armonía, orquestación, teoría modal
- Visualización de transiciones modales y lógica escalar

🔭 **Uso en Investigación**
- Comparación entre estrategias generativas
- Análisis de modulación modal
- Integración con IA simbólica (planificación, sistemas expertos)
- Modelado de percepción y adaptación en tiempo real

⚠️ **Limitaciones (Alcance Actual)**

- Solo admite escalas de 7 notas (no pentatónicas ni octatónicas)  
- Compás fijo por sección (sin cambios internos de métrica)  
- Sin retroalimentación desde la percepción del audio ni aprendizaje por preferencia  
- El motor MIDI/audio limita el realismo de la articulación humana

El motor de modulación actualmente admite más de 1.000 modos y realiza más de un millón de comparaciones por pares de forma eficiente, mediante matrices de similitud precalculadas.  
Aunque este diseño escala bien dentro del conjunto armónico actual, futuras expansiones a varios miles de modos podrían beneficiarse de optimizaciones basadas en índices para mantener el rendimiento.

Aunque el motor de modulación admite más de 1.000 modos y millones de comparaciones por pares, la escalabilidad futura con conjuntos modales aún más grandes podría beneficiarse de optimizaciones por índices.  
La implementación actual abarca múltiples archivos, con un script centralizado que orquesta la mayoría de la lógica armónica, rítmica y melódica en un único flujo integrado.  
Aunque armonía, ritmo y melodía no están completamente separados como módulos independientes, el código sigue una lógica estructurada que facilita actualizaciones, experimentación y extensión de funcionalidades.  
Una desacoplamiento adicional podría permitir en el futuro una modularización más clara y el intercambio dinámico de subsistemas para control en tiempo real o estrategias alternativas.

🌐 **Sitio Web y Acceso**

🔗 https://reharmonizationmaps.com  
Explorá el mapa interactivo de escalas, analizá intersecciones modales y generá canciones completas.

🧠 **Nota Final**

RM Song Generator no está diseñado para simular a un compositor humano —sino para mostrar lo que puede crear un algoritmo informado, con teoría musical explícita, memoria tonal, conciencia estructural y lógica interna.  
Contribuye a un campo de creatividad computacional donde claridad, estructura y emoción pueden coexistir.

▶️ [Descargar video demo](https://github.com/Fabri-D/RM-Song-Generator/blob/main/videosonggenerator.webm)
