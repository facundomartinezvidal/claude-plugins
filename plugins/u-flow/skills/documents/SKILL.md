---
name: u-flow:documents
description: This skill should be used when the user needs to write, structure, or improve written academic documents for university: informes, trabajos prácticos, ensayos, monografías, or parciales domiciliarios. Triggers on phrases like "ayudame a escribir un informe", "redactar un trabajo práctico", "estructura para mi TP", "consigna del trabajo práctico", "monografía sobre", "parcial domiciliario", "corregime la redacción", "mejorame el estilo", "TP integrador", "tengo que entregar un escrito", or when the user pastes a consigna and needs help producing a written response. Looks for the consigna and lineamientos (usually a PDF in the workspace) before asking. For slide content use u-flow:presentations instead. Responds in Spanish by default.
allowed-tools:
  - Read
  - Glob
  - Grep
  - Write
  - EnterPlanMode
  - ExitPlanMode
---

# Documents — Academic Written Work

## Purpose

To help students write well-structured academic documents: informes, trabajos prácticos, ensayos, monografías, and parciales domiciliarios. Apply when the user needs to produce formal academic writing, with or without a specific assignment prompt (consigna).

## Core Principles

- Respond in **Spanish** unless the user explicitly requests another language
- Always read the consigna carefully — structure must match what is asked
- Academic tone: formal but not unnecessarily complex
- Cite sources when the user provides them; flag when citations are needed but absent
- Adapt format to discipline (humanities, sciences, social sciences differ significantly)
- This skill writes documents; for slides hand off to u-flow:presentations

## Finding the Consigna

The consigna and lineamientos are usually a **PDF the user already has** — do not ask for them before looking:

1. **Check the conversation** — did the user paste or attach the consigna?
2. **Search the workspace** — Glob for `**/*.pdf`, `**/*.docx`, `**/*.md` and Grep for terms like `consigna`, `trabajo práctico`, `TP`, `lineamientos`, `pautas`, `entrega`, `rúbrica`, or the subject name. Files named like `consigna.pdf`, `tp1.pdf`, `lineamientos-entrega.pdf` are prime candidates
3. **Read what you find** and confirm with the user: "Encontré `[archivo]` — ¿es la consigna de este trabajo?" For large PDFs or several candidates, delegate to the `u-flow:reader` agent — it reads in an isolated context and returns the consigna, lineamientos, and relevant extracts
4. **Only if nothing is found**, ask the user for the consigna or offer to work with the standard structure

## Working With the Consigna

Once you have the consigna (found or provided):
1. **Parse the consigna** — identify: topic, required sections, word count, format, citation style
2. **Extract the lineamientos** — formatting and submission rules often live in the same PDF or a separate one: extensión mínima/máxima, tipografía y tamaño, interlineado, carátula obligatoria, formato de entrega, criterios de evaluación o rúbrica
3. **Honor every explicit requirement** — lineamientos override all default structures and guidelines in this skill
4. **Clarify if ambiguous** — ask one focused question if critical details are missing
5. **Enter plan mode** — use EnterPlanMode and build the document plan there
6. **Present the plan** — outline with every section, noting which consigna requirement each one answers, expected length per section, and citation style. Exit plan mode for approval via ExitPlanMode
7. **Write only after approval** — generate full text section by section
8. **Review before submission** — when the draft is complete, delegate to the `u-flow:reviewer` agent: it re-reads the cátedra's material and verifies every attributed claim, plus consigna/lineamientos/rúbrica compliance, and returns findings by severity. Offer: "¿Hago una revisión final contra el material y la consigna antes de que lo entregues?"

## Plan Mode

Always plan before writing — a document written against the wrong structure wastes the student's time:
- After discovering the consigna and material, call **EnterPlanMode**
- The plan must show: estructura completa, qué requisito de la consigna cubre cada sección, extensión estimada por sección, estilo de citado, fuentes a usar
- Call **ExitPlanMode** to present the plan; write nothing until the user approves
- If the user rejects or adjusts, revise the plan and present again
- Skip plan mode only for trivial requests: single-section fixes, "corregime la redacción", style revisions of an existing draft

## Academic Report Structure

### Standard University Report

```
# [Título del Trabajo]
**Asignatura:** [Materia]  
**Autor/a:** [Nombre]  
**Fecha:** [Fecha]

---

## Introducción
[150-300 palabras]
- Contexto del tema
- Objetivo del trabajo
- Estructura del documento

## Desarrollo
### [Subtema 1]
[Contenido con argumentación]

### [Subtema 2]
[Contenido con argumentación]

### [Subtema N]
...

## Conclusión
[150-250 palabras]
- Síntesis de los puntos principales
- Respuesta a la pregunta o hipótesis inicial
- Reflexión final o implicancias

## Bibliografía
[Lista de fuentes en formato APA 7ª edición — default salvo que la consigna indique otro estilo]
```

### Scientific / Technical Report

For lab reports or technical documents:

```
Introducción: contexto y objetivo del experimento/trabajo
Objetivos: qué se busca demostrar o medir
Marco Teórico: fundamentos y conceptos aplicados
Metodología: procedimiento, materiales, variables
Resultados: datos obtenidos (tablas, gráficos)
Discusión: interpretación de los resultados
Conclusiones: síntesis y respuesta a los objetivos
Referencias: APA 7 por defecto
```

### Parcial Domiciliario (take-home exam)

A timed take-home exam — usually analytical, essay-style, no formal cover page or bibliography unless the consigna asks for one:
- Answer each question directly; lead with the thesis, then justify
- Prioritize argumentation and integration of course material over length
- Respect the per-question word limit if given
- Cite course readings inline if the consigna requires it; otherwise reference authors by name

### Monografía

For extended research papers (monografías), use this structure:

```
Carátula: título, autor, materia, docente, institución, fecha
Índice: secciones con numeración de página
Introducción: presentación del problema, justificación, objetivos, hipótesis (si aplica)
Marco Teórico: estado del arte, conceptos clave, autores de referencia
Desarrollo: análisis temático organizado en capítulos o secciones
Conclusión: respuesta a los objetivos, síntesis del argumento, aporte del trabajo
Bibliografía: APA 7 por defecto, ordenada alfabéticamente
```

Ask the user for: extensión requerida (páginas), si tiene consigna específica, fuentes disponibles. If no sources are available, offer to research first: "¿Querés que investiguemos el tema antes de empezar a escribir? Podés usar `/u-flow:research` para reunir fuentes."

## Writing Process

1. **Analyze consigna** — extract requirements, scope, expected length
2. **Gather content** — ask user for notes, sources, or topic details if not provided
3. **Draft outline in plan mode** — present structure for approval (see Plan Mode)
4. **Write section by section** — introduce → develop → conclude each section
5. **Check coherence** — ensure transitions between sections flow naturally
6. **Add bibliography placeholder** — note where citations should appear

## Academic Tone Guidelines

**Use:**
- Third person when appropriate: "Se puede observar que...", "Los resultados indican..."
- Precise vocabulary: define technical terms on first use
- Hedging language when appropriate: "Se podría considerar...", "Esto sugiere que..."
- Logical connectors: "Por otro lado", "En consecuencia", "Sin embargo", "A diferencia de"

**Avoid:**
- First person singular unless required: "Yo creo", "Me parece"
- Colloquial language: contractions, slang
- Unsupported assertions: every claim should link to evidence or reasoning
- Repetition of the same phrase or structure in consecutive sentences

## Presentations

This skill covers written documents. For slide content and speaker notes, use the dedicated `u-flow:presentations` skill (command: `/u-flow:presentation`). If the user asks to turn a report into a deck, hand off: "¿Querés que arme las diapositivas? Usá `/u-flow:presentation`."

## Citation Handling

If the user provides sources:
- Format citations in APA 7th edition by default unless another style is specified
- In-text: (Apellido, año) or (Apellido, año, p. X) for direct quotes
- Reference list: alphabetical by first author's last name

If no sources are provided:
- Flag where citations are needed: "[CITA NECESARIA]"
- Offer to find them: "¿Querés que busque fuentes para este punto? Podés pedirme `/u-flow:research [tema]`"

## Calibrating Output

- **Outline only**: when user wants to plan before writing
- **Full draft**: when user provides enough content and approves structure
- **Single section**: when user needs help with one part
- **Revision**: when user pastes their own draft and asks for feedback or improvement

## Saving Output

Deliver in chat by default. When the user wants the result saved (or accepts the offer to save it):
- Location: `[materia]/entregas/` — follow the Estructura in the materia's CLAUDE.md if it defines a different layout
- Name: `[tipo]-[tema].md`, kebab-case, no accents — e.g. `entregas/tp2-ciclo-economico.md` — deliverables, not study material
- If the same TP accumulates 2+ files (informe + presentacion + anexos), group them: `entregas/tp2/informe.md`, `entregas/tp2/presentacion.md` — move the existing loose file in when creating the second
- Create the folder if it doesn't exist; never overwrite an existing file without confirming

## Quality Check

Before finalizing:
- The workspace was searched for the consigna/lineamientos before asking the user
- Every section from the consigna is addressed
- Every explicit lineamiento is honored: extensión, formato, carátula, estilo de citado
- If a rúbrica exists, each evaluation criterion is covered
- Introduction states the objective clearly
- Conclusion answers what the introduction promised
- No section is just a list — each has at least one developed paragraph
- Tone is consistent throughout
