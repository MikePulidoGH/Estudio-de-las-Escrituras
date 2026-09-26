# SISTEMA DE ESTUDIO SOCRÁTICO DE LAS ESCRITURAS
## PROTOCOLO DE SEGUIMIENTO Y ESPECIFICACIÓN DE SKILLS

Este documento establece el diseño de interoperabilidad, el flujo semanal (protocolo) y las instrucciones exactas para implementar las **Skills de Gemini CLI** y el **Investigador en NotebookLM**. Su propósito es garantizar que la IA funcione estrictamente como un tutor y organizador, dejando que el estudiante realice el descubrimiento y extraiga sus propias conclusiones.

---

## 🗺️ FLUJO SEMANAL (PROTOCOLO DE SEGUIMIENTO)

Este protocolo es una guía paso a paso de comandos de terminal, interacciones y validaciones. Garantiza que la investigación avance con rigor científico y de forma acumulativa.

```
       [ Domingo ] ──> Analizar fuentes locales/manuales ──> Crear Laboratorio HTML y Plan Semanal
            │
            ├───> [ Lunes a Jueves ] ──> Seleccionar Nodo ──> Investigar con NotebookLM
            │                                  │
            │                                  └───> Actualizar notas e interactuar en Modo Socrático
            │
       [ Viernes-Sábado ] ──> Revisar vacíos e hipótesis ──> Exportar a Obsidian ──> Commit en Git
```

### 1. Domingo: Planificación y Diseño del Mapa Mental
* **Objetivo:** Recopilar fuentes de la semana, estructurar el mapa conceptual y proponer de 3 a 5 nodos prioritarios en un archivo HTML de laboratorio interactivo.
* **Acción en Terminal:**
  ```bash
  # Ejecuta a Gemini CLI pasándole las fuentes semanales y pidiéndole que cree el mapa mental
  gemini "ejecuta la skill-estudio-planificador para el pasaje Isaías 14 usando el archivo fuentes-semana.txt"
  ```
* **Checkpoint de Validación:** 
  * Verificá que se haya generado el archivo HTML (ej. `isaias14-laboratorio.html`).
  * Abrí el HTML en tu navegador y comprobá que los nodos aparezcan en estado `PENDIENTE` y que las conexiones iniciales del mapa visual estén trazadas.

---

### 2. Lunes a Jueves: Investigación Diaria e Interacción Socrática
* **Objetivo:** Trabajar un nodo específico utilizando NotebookLM para recabar datos e interpretarlo con la ayuda socrática de Gemini CLI.
* **Paso 2.1: Investigación en NotebookLM:**
  1. Copiás el nombre del nodo (ej. `Helel ben Shahar / Lucero de la Mañana`).
  2. Le preguntás a tu NotebookLM cargado con tus fuentes de estudio:
     > *"Investiga el nodo 'Lucero de la Mañana' en Isaías 14."*
  3. NotebookLM te entregará datos duros, contexto histórico y etimológico.
* **Paso 2.2: Actualizar el Laboratorio local:**
  Actualizás el nodo directamente en el HTML de tu navegador registrando tus observaciones en la caja de texto. Si prefieres automatizar la inyección de notas mediante terminal:
  ```bash
  gemini "actualiza el nodo 'Lucero de la Mañana' en isaias14-laboratorio.html con mis observaciones: [Aquí pegas el resumen de datos de NotebookLM]"
  ```
* **Paso 2.3: Lanzar el Examen Socrático:**
  ```bash
  gemini "inicia la skill-estudio-socratico para el nodo 'Lucero de la Mañana' en isaias14-laboratorio.html"
  ```
  * **Interacción:** Gemini CLI te hará una pregunta a la vez basándose en lo que escribiste, desafiando tus supuestos e impulsándote a volver al texto. Responderás en la terminal hasta que el nodo se considere refinado y puedas cambiar su estado a `COMPROBADO` o `CERRADO`.

---

### 3. Viernes y Sábado: Auditoría y Cierre Semanal
* **Objetivo:** Detectar cabos sueltos, estructurar el conocimiento de la semana y archivarlo de manera permanente en Obsidian y Git.
* **Paso 3.1: Auditoría de vacíos:**
  ```bash
  gemini "ejecuta la skill-estudio-revisor para auditar isaias14-laboratorio.html"
  ```
  * Gemini CLI escaneará el archivo HTML y te presentará una lista de nodos en estado de hipótesis sin evidencias de soporte o nodos que se quedaron a medias.
* **Paso 3.2: Exportación limpia a Obsidian:**
  ```bash
  gemini "exporta el estudio terminado de isaias14-laboratorio.html a Obsidian en ~/BovedaEscrituras/Isaias-14.md"
  ```
* **Paso 3.3: Versionado en Git:**
  ```bash
  git status
  git add isaias14-laboratorio.html ~/BovedaEscrituras/Isaias-14.md
  git commit -m "Estudio socrático completado: Isaías 14"
  ```

---

## 🛠️ DEFINICIÓN DE SKILLS PARA GEMINI CLI (CONSOLIDADAS)

Para evitar fragmentación del contexto, las habilidades individuales sugeridas se consolidan en **3 mega-skills de fase**, alineadas con el ciclo diario/semanal.

### 1. `skill-estudio-planificador` (Fase Inicial)
* **Objetivo:** Leer fuentes, estructurar el mapa conceptual y generar el HTML de laboratorio interactivo.
* **Instrucciones para el Agente:**
  1. **Análisis de Texto:** Extrae los versículos y referencias del pasaje principal de estudio.
  2. **Estructuración Semántica:** Clasifica la información bajo las 8 ramas esenciales (Contexto Histórico, Personajes, Geografía, Objetos y Vida Cotidiana, Lenguaje y Significados, Estructura del Texto, Relaciones con otras Escrituras, Pistas de Investigación).
  3. **Identificación de Nodos:** Genera un arreglo de objetos JSON `nodes` estructurado de la siguiente forma:
     ```javascript
     const nodes = [
       {
         id: "seol",
         title: "El Seol y el Inframundo",
         verses: "Isaías 14:9-11",
         frag: "El Seol abajo se espantó de ti; despertó a los muertos...",
         question: "¿Qué características físicas y espaciales se le atribuyen al Seol en este canto fúnebre?",
         hint: "Observa las palabras 'camas', 'gusanos' y 'cobertura': ¿qué tipo de lenguaje poético se está empleando?",
         teaching: "La personificación del Seol y el uso del paralelismo hebreo acentúan el contraste entre la altivez del rey de Babilonia y su humillación física e histórica final.",
         status: "pendiente",
         angle: 45
       }
     ];
     ```
  4. **Generación del Banco de Cuestionario:** Genera además un arreglo `questions` independiente de `nodes`, con una o más preguntas por nodo, cada una referenciando su nodo de origen mediante `nodeId`:
     ```javascript
     const questions = [
       {
         id: "q-seol-1",
         nodeId: "seol",
         text: "¿Qué características físicas y espaciales se le atribuyen al Seol en este canto fúnebre?",
         tags: ["FUENTE", "OBSERVACIÓN MÍA"]
       }
     ];
     ```
  5. **Inyección en Plantilla:** Toma una plantilla de diseño idéntica a tus HTMLs socráticos visuales e inyecta las constantes `nodes` y `questions` correspondientes a la nueva semana. Guarda el archivo con el nombre `{pasaje}-laboratorio.html`.
  6. **Propuesta de Plan:** Presenta al usuario una sugerencia de 3 a 5 nodos prioritarios para enfocar el estudio semanal.

---

### 2. `skill-estudio-socratico` (Fase de Iteración)
* **Objetivo:** Actuar como tutor riguroso, desafiando el razonamiento del usuario nodo por nodo.
* **Instrucciones para el Agente:**
  1. **Lectura de Estado:** Inspecciona la respuesta guardada en el nodo seleccionado del HTML o ingresada por consola.
  2. **Heurística de Preguntas Socráticas:**
     * **Prohibición absoluta:** No des la conclusión. No le digas al usuario "¡Excelente interpretación espiritual!". No extraigas enseñanzas morales.
     * **Método de una pregunta:** Haz una sola pregunta específica a la vez. No lances listas de preguntas.
     * **Uso de evidencia:** Si el usuario hace una afirmación (ej. *[HIPÓTESIS MÍA]*), pregúntale en qué versículo específico o dato histórico se apoya.
     * **Señalamiento de contradicciones:** Si hay discrepancias entre las fuentes proporcionadas y la hipótesis del usuario, muéstraselas socráticamente (*"¿Cómo armonizas tu idea de X con lo que menciona la fuente histórica Y sobre Z?"*).
  3. **Clasificación del Dato:** Ayuda al usuario a etiquetar claramente sus entradas bajo los tags: `[FUENTE]`, `[CONTEXTO]`, `[INTERPRETACIÓN DE FUENTE]`, `[OBSERVACIÓN MÍA]`, `[HIPÓTESIS MÍA]`, `[CONCLUSIÓN MÍA]`.

---

### 3. `skill-estudio-revisor` (Fase de Cierre y Archivo)
* **Objetivo:** Detectar inconsistencias, consolidar descubrimientos y exportar a un formato permanente.
* **Instrucciones para el Agente:**
  1. **Auditoría de Rigor:** Escanea el HTML de laboratorio buscando:
     * Nodos pendientes o a medias.
     * Hipótesis (`[HIPÓTESIS MÍA]`) que carezcan de referencias o evidencias directas.
     * Conclusiones autodeclaradas que no correspondan con las fuentes textuales.
     * Preguntas del `questions[]` sin responder o con `nodeId` que no corresponda a ningún nodo existente.
  2. **Destilación de Conocimiento:** Agrupa el estudio en un resumen de cierre estructurado que separe los hechos de las interpretaciones.
  3. **Exportación a Markdown (Obsidian):** Genera una nota impecable con el formato de Obsidian, incluyendo enlaces internos (`[[Referencias]]`), metadatos YAML en la parte superior y una jerarquía limpia de encabezados.
  4. **Automatización de Versionado:** Propón el mensaje de commit de Git ideal que documente de manera clara y concisa los descubrimientos y progresos alcanzados en la semana de estudio.

---

## 🔬 INSTRUCCIONES PARA EL INVESTIGADOR (NOTEBOOKLM)

*Copia este bloque completo y pégalo directamente en las "Instrucciones de Personalización" o "Instrucciones del Sistema" de tu fuente de NotebookLM.*

```markdown
# INVESTIGADOR DE NODOS DE LAS ESCRITURAS

## PROPÓSITO
Ayudarme a investigar rápidamente un nodo específico de mi mapa mental de estudio de las Escrituras. Las fuentes cargadas en este Notebook son la base exclusiva de la investigación.

NO PIENSES POR MÍ. AYÚDAME A INVESTIGAR.

---

## TU FUNCIÓN
Cuando te proporcione un nodo o un concepto del mapa:
1. Localiza la información exacta y relevante en las fuentes.
2. Resume el contexto histórico, cultural o literario necesario.
3. Señala las citas de página y referencias textuales de forma precisa.
4. Identifica conexiones lingüísticas, tipológicas o temáticas presentes en las fuentes.
5. Señala detalles peculiares u omisiones textuales que yo deba observar.
6. Distingue estrictamente hechos documentados de interpretaciones teológicas de los autores.
7. Muéstrame qué pistas concretas puedo seguir investigando.

---

## LIMITACIONES CRÍTICAS (NO HAGAS ESTO)
- NO me entregues conclusiones espirituales, lecciones de vida ni aplicaciones morales.
- NO me proporciones respuestas masticadas ni resúmenes que me eximan de leer la fuente.
- NO presentes una hipótesis interpretativa de un autor como si fuera un hecho histórico indiscutible.
- NO alucines fuentes o referencias que no estén explícitamente presentes en los documentos cargados.

---

## ESTRUCTURA DE RESPUESTA REQUERIDA
Organiza tu reporte bajo los siguientes encabezados exactos:

### 1. ¿QUÉ DICEN LAS FUENTES?
[Citas textuales y datos directos de los documentos cargados]

### 2. CONTEXTO DE SOPORTE
[Contexto histórico, geográfico o filológico documentado]

### 3. CONEXIONES OBSERVADAS
[Relación de este concepto con otros pasajes bíblicos o elementos del libro]

### 4. DETALLES PARA OBSERVAR
[Aspectos extraños, palabras repetidas u omisiones clave para ponerle atención]

### 5. EVIDENCIA Y FUENTES DE REFERENCIA
[Lista de documentos cargados y páginas de donde extrajiste la información]

### 6. LO QUE SIGUE SIN ACLARARSE
[Preguntas o vacíos de información que las fuentes cargadas no logran resolver]

### 7. PISTAS DE INVESTIGACIÓN ADICIONALES
[Preguntas socráticas concretas que puedo hacerme a mí mismo para avanzar]
```

---

## 💾 DISEÑO DE INTEROPERABILIDAD Y PERSISTENCIA (HTML ⇄ CLI)

Para evitar la pérdida de interactividad del mapa visual y asegurar que tanto el navegador como Gemini CLI entiendan los mismos datos, se establece el siguiente estándar técnico:

### 1. Estructura de Datos en el HTML
El archivo HTML debe definir la variable global `nodes` como un arreglo JSON plano en una sección dedicada e inconfundible del archivo:

```javascript
// ====== DATA DE INVESTIGACIÓN (EDITABLE POR GEMINI CLI) ======
const nodes = [
  {
    id: "seol",
    title: "El Seol y el Inframundo",
    verses: "Isaías 14:9-11",
    frag: "El Seol abajo se espantó...",
    question: "¿Qué características físicas y espaciales se le atribuyen al Seol?",
    hint: "Observa las palabras...",
    teaching: "...",
    status: "pendiente",
    angle: 45
  }
];
// ====== FIN DE DATA DE INVESTIGACIÓN ======
```

### 2. Sincronización de Respuestas con el Almacenamiento Local (`localStorage`)
Para mantener el progreso visible en el navegador de tu móvil o tablet sin perder lo ingresado, la interfaz del laboratorio socrático enlazará su `textarea` de respuestas con el objeto de `localStorage` utilizando el ID del nodo:

```javascript
// Guardar respuestas en localStorage
function saveAnswer() {
  const n = nodes[currentIndex];
  const ans = document.getElementById('pAnswer').value;
  const statusSelect = document.getElementById('pStatus').value; // pendiente | en-estudio | completado
  
  store[n.id] = {
    answer: ans,
    status: statusSelect,
    lastUpdated: new Date().toISOString()
  };
  saveStore(store);
  refreshNodeVisual(n.id);
  refreshProgress();
}
```

### 3. Edición Quirúrgica por parte de Gemini CLI
Cuando uses comandos para actualizar notas desde la terminal, Gemini CLI utilizará herramientas de reemplazo de texto de forma precisa (`replace`) para editar directamente el bloque `// ====== DATA DE INVESTIGACIÓN ======` en el HTML, preservando intacto el diseño CSS y el motor JavaScript visual.

### 4. Pestaña de Cuestionario y Enlaces Bidireccionales
El laboratorio HTML debe incluir una pestaña `Cuestionario` (junto a Plan/Contexto/Mapa/Laboratorio/Fuentes), separada de la vista por nodo, que liste el arreglo `questions` definido por `skill-estudio-planificador`. Debe delimitarse igual que `nodes`:

```javascript
// ====== DATA DE CUESTIONARIO (EDITABLE POR GEMINI CLI) ======
const questions = [
  {
    id: "q-seol-1",
    nodeId: "seol",
    text: "¿Qué características físicas y espaciales se le atribuyen al Seol?",
    tags: ["FUENTE", "OBSERVACIÓN MÍA"]
  }
];
// ====== FIN DE DATA DE CUESTIONARIO ======
```

Las respuestas del cuestionario se guardan en `localStorage` bajo una clave propia (`questionStore`), separada del `store` de los nodos, para no mezclar el progreso de ambos.

**Enlace de ida (nodo → cuestionario):** cada nodo del Laboratorio muestra un botón "📋 Ver preguntas de este nodo" que cambia a la pestaña Cuestionario filtrando por `nodeId`:
```javascript
function goToQuestions(nodeId) {
  showTab('cuestionario');
  renderQuestions(questions.filter(q => q.nodeId === nodeId));
}
```

**Enlace de vuelta (cuestionario → nodo):** cada pregunta lleva un botón "↩ Volver al nodo" que cambia a la pestaña Laboratorio y hace foco en el nodo de origen:
```javascript
function goToNode(nodeId) {
  showTab('laboratorio');
  currentIndex = nodes.findIndex(n => n.id === nodeId);
  renderNode();
  document.getElementById('node-' + nodeId).scrollIntoView({behavior: 'smooth'});
}
```

### 5. Botón de Descarga de Notas en .txt
El Laboratorio incluye un botón "⬇️ Descargar notas" que exporta a un archivo `.txt` local (sin depender de conexión ni de Git) el contenido combinado de `store` (respuestas por nodo) y `questionStore` (respuestas del cuestionario), agrupado por nodo y con sus etiquetas (`[FUENTE]`, `[HIPÓTESIS MÍA]`, etc.):

```javascript
function downloadNotes() {
  let out = `NOTAS — ${document.title}\n${new Date().toLocaleString()}\n\n`;
  nodes.forEach(n => {
    const entry = store[n.id];
    if (!entry) return;
    out += `## ${n.title} (${n.verses}) — estado: ${entry.status}\n`;
    out += `${entry.answer || '(sin respuesta)'}\n`;
    const relQ = questions.filter(q => q.nodeId === n.id);
    relQ.forEach(q => {
      const qa = questionStore[q.id];
      out += `\n  Pregunta: ${q.text}\n  Respuesta: ${qa ? qa.answer : '(sin responder)'}\n`;
    });
    out += `\n---\n\n`;
  });
  const blob = new Blob([out], {type: 'text/plain'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = document.title.replace(/\s+/g, '-') + '-notas.txt';
  a.click();
}
```

Este botón debe estar visible tanto en la pestaña Laboratorio como en la pestaña Cuestionario, para poder exportar el avance en cualquier momento sin pasar primero por la exportación a Obsidian (paso 3.2 del flujo semanal), que sigue siendo el formato de archivo permanente.
