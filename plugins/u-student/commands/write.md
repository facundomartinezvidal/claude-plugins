---
name: write
description: Write an academic document section by section — TP, informe, ensayo, monografía, or reseña. Reads assignment guidelines, enters plan mode to define structure and format, then writes each section in depth waiting for approval before continuing.
argument-hint: "[topic or assignment guidelines file]"
allowed-tools: Read, WebFetch, Bash, Write, EnterPlanMode
---

Write an academic document for a student, section by section. Never generate the full document at once — always write one section, wait for approval, then continue.

## Setup

Run workflow-core steps W1, W2, W4.
Use PLAN_MODE from W4 for the plan step below.
(Skip W3 missing-content check — not needed for document writing.)

## Step 1 — Get assignment guidelines

If no guidelines were provided, ask:
"¿Tenés la consigna o los lineamientos del trabajo? Es importante para estructurar el documento según lo que pide el profesor. Podés pegarla o compartir el archivo."

If the student doesn't have guidelines, ask:
- "¿Qué tipo de trabajo es? (TP, informe, ensayo, monografía, reseña)"
- "¿Tenés algún criterio de evaluación o rúbrica?"
- Proceed with a generic structure for the chosen document type.

## Step 2 — Enter plan mode

Present this plan to the student:

```
## Plan del documento

**Tipo de documento detectado:** [inferred or ask]
**Basado en:** [guidelines file / pasted guidelines / generic structure]

**Opciones a confirmar:**

1. ¿Tipo de documento?
   - [ ] Trabajo Práctico (TP)
   - [ ] Informe de laboratorio
   - [ ] Ensayo argumentativo
   - [ ] Monografía
   - [ ] Reseña bibliográfica
   - [ ] Otro: ___

2. ¿Formato de salida?
   - [ ] Markdown (.md)
   - [ ] Word (.docx)
   - [ ] Ambos

3. ¿Extensión aproximada?
   - [ ] Corto (1–3 páginas)
   - [ ] Medio (4–8 páginas)
   - [ ] Largo (9+ páginas)

4. ¿Incluir referencias bibliográficas?
   - [ ] Sí — voy a proveer las fuentes
   - [ ] Sí — indicame dónde buscar
   - [ ] No es necesario

**Estructura propuesta:**
[Generate sections based on guidelines — if guidelines exist, map them directly;
if not, use standard structure for the chosen document type]

| # | Sección | Basada en | Requiere input del estudiante |
|---|---------|-----------|-------------------------------|
| 1 | Introducción | [guideline point X] | No |
| 2 | [Section] | [guideline point Y] | No |
| … | … | … | … |
| N | Conclusión | [guideline point Z] | No |
| — | Referencias | — | Sí — el estudiante provee fuentes |

Secciones marcadas como "Sí" en la última columna serán dejadas con `[COMPLETAR]`
hasta que el estudiante provea la información necesaria.
```

If guidelines exist, cross-check every required point against the proposed structure.
Flag any guideline requirement not covered.

Wait for student approval or adjustments.

## Step 6 — Write section by section

For each section in the approved structure:

### 6a — Announce the section
Tell the student:
```
## Sección [N] — [Section Name]

[Brief description of what this section will cover and why it matters in the document]

Escribiendo...
```

### 6b — Write the section
Write the full section in depth:
- Use the course's terminology from the material
- Build arguments logically — claim → evidence → explanation
- Connect explicitly to the previous section where relevant
- Use formal academic Spanish
- Do not fabricate data, statistics, or citations
- If this section requires personal data, experiments, or the student's own opinion:
  mark with `[COMPLETAR: descripción de lo que falta]` and explain what the student needs to add

### 6c — Wait for approval
After each section ask:
```
¿Continuamos con la siguiente sección ("[Next Section Name]") o querés ajustar algo de esta?
```

Do NOT proceed until the student confirms.

### 6d — Handle [COMPLETAR] sections
If a section is entirely dependent on student input (e.g., experimental results, personal reflection):
- Provide the structure and headings
- Write the introductory sentence
- Leave the body as `[COMPLETAR: explicá aquí X]`
- Suggest what kind of content goes there
- Ask the student if they want to provide it now or continue and come back later

## Step 7 — Compile final document

After all sections are approved:
- Combine all sections into a single document
- Add a proper header: title, subject name, student name placeholder `[Nombre]`, date placeholder `[Fecha]`
- Add page break markers between major sections
- Save as `{kebab-case-topic}.md`

If student requested .docx, check for pandoc:
```bash
which pandoc 2>/dev/null
```
If available:
```bash
pandoc {file}.md -o {file}.docx
```
If not available, tell the student:
```
No encontré pandoc. Instalá con: brew install pandoc
O copiá el .md en Google Docs o Word.
```

To export to PDF, use:
```
/u-student:export {file}.md
/u-student:export {file}.docx
```

## Step 8 — Final check against guidelines

If guidelines were provided, run a final verification:
- List every required point from the guidelines
- Confirm which sections address each point
- Flag anything not covered or that has `[COMPLETAR]` still pending

```
## Verificación de consigna

| Requisito | Cubierto en | Estado |
|-----------|-------------|--------|
| [req 1]   | Introducción | ✅ |
| [req 2]   | Sección 3    | ✅ |
| [req 3]   | —            | ⚠️ COMPLETAR |
```

Tell the student:
```
✅ Documento guardado: {topic}.md [y/o {topic}.docx]

[if any COMPLETAR remaining:]
⚠️ Quedan [N] secciones por completar con tu información personal. Revisalas antes de entregar.

📄 Exportar a PDF: /u-student:export {topic}.md

💡 Próximos pasos:
  (writing-coach agent)    — si necesitás ayuda para redactar las secciones [COMPLETAR]
  (research-assistant agent) — si necesitás fuentes para respaldar afirmaciones
  (tp-reviewer agent)      — revisión completa antes de entregar
```
