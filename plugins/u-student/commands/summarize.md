---
name: summarize
description: Summarize academic content for a subject — accepts text, files, images, PDFs, URLs, or transcriptions. Checks the subject cronograma to track coverage, then enters plan mode so the student defines format and depth before generating the summary.
argument-hint: "[files, text, or URLs to summarize]"
allowed-tools: Read, WebFetch, Bash, EnterPlanMode
---

Generate a summary of academic content for a student. Follow this workflow precisely.

## Setup

Run workflow-core steps W1, W2, W3, W4.
Use PLAN_MODE from W4 for the plan step below.

## Step 1 — Enter plan mode

Present this plan to the student for approval:

```
## Plan de resumen

**Contenido encontrado:**
- [list sources with class number and partial]

**Cobertura según cronograma:**
- Clases cubiertas: [list]
- Clases faltantes: [list or "cronograma no disponible"]

**Opciones a confirmar:**

1. ¿Qué querés resumir?
   - [ ] Todo el contenido provisto
   - [ ] Solo primer parcial
   - [ ] Solo segundo parcial
   - [ ] Clases específicas: ___

2. ¿Formato del resumen?
   - [ ] Estándar (tema + conceptos clave + desarrollo + preguntas)
   - [ ] Solo bullets
   - [ ] Desarrollo extendido
   - [ ] Por clase (un resumen por clase)
   - [ ] Por unidad del programa

3. ¿Profundidad?
   - [ ] Corto (puntos esenciales)
   - [ ] Medio (explicación de cada concepto)
   - [ ] Largo (con ejemplos y contexto)

4. ¿Incluir preguntas de autoevaluación?
   - [ ] Sí, abiertas
   - [ ] Sí, múltiple choice
   - [ ] No

5. ¿Idioma del resumen?
   - [ ] Español
   - [ ] Inglés
   - [ ] Mismo idioma que el material
```

Wait for student approval or adjustments before proceeding.

## Step 6 — Generate summary

Execute according to the approved plan. Structure the output based on the chosen format.

Always prioritize the student's own course material over general knowledge. If a concept appears in the material, summarize it from there — do not substitute with external definitions unless the material is unclear or incomplete.

If content for some classes is missing and the student chose to summarize the full subject, note it clearly:
> ⚠️ Clase 04 — "Transformada Z": no se encontró material. Verificá si falta subir el archivo.
