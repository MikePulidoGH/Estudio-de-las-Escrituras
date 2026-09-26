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

## 🛠️ DEFINICIÓN DE SKILLS PARA GEMINI CLI

> **La especificación completa y actualizada de `skill-estudio-planificador`, `skill-estudio-socratico` y `skill-estudio-revisor` — incluyendo el esquema de campos de `nodes`/`questions`, la función `urlFuente()`, las reglas de evidencia y el estándar de interoperabilidad HTML ⇄ CLI (persistencia en `localStorage`, pestaña Cuestionario, enlaces bidireccionales y descarga de notas) — vive en [`skills-completas-estudio-escrituras.md`](./skills-completas-estudio-escrituras.md).** Este archivo (`protocolo-estudio.md`) se limita al flujo semanal de trabajo y al prompt del Investigador en NotebookLM; no dupliques aquí la definición de skills.

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

> El estándar técnico completo (estructura de `nodes`/`questions` en el HTML, sincronización con `localStorage`, edición quirúrgica por Gemini CLI, pestaña Cuestionario con enlaces bidireccionales y botón de descarga de notas en `.txt`) también vive en [`skills-completas-estudio-escrituras.md`](./skills-completas-estudio-escrituras.md), dentro de `skill-estudio-planificador`. Consultalo ahí para no mantener dos copias del mismo código.
