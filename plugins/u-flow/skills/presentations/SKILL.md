---
name: u-flow:presentations
description: This skill should be used when the user wants to create slide content and speaker notes for a university presentation or oral defense. Triggers on phrases like "armar una presentación", "hacer las diapositivas", "contenido para las slides", "preparar la exposición", "tengo que exponer", "presentación para la clase", "diapositivas sobre", "defensa del trabajo", or when the user needs to turn a topic, report, or notes into a structured deck to present. Looks for the consigna/lineamientos and class material (usually PDFs in the workspace) before asking. Produces slide-by-slide markdown with bullets and speaker notes, not a rendered file. Responds in Spanish by default.
allowed-tools:
  - Read
  - Glob
  - Grep
  - Write
  - EnterPlanMode
  - ExitPlanMode
---

# Presentations — University Slide Content

## Purpose

To turn a topic, report, or notes into structured slide-by-slide content with speaker notes, ready for a university presentation or oral defense. Apply when the user needs to present, expose, or defend academic work.

Output is **markdown content**, not a rendered file. The student copies each slide into PowerPoint, Google Slides, Canva, or any tool. If the user wants a rendered deck, offer a Marp markdown version (see Marp Fallback).

## Core Principles

- Respond in **Spanish** unless the user explicitly requests another language
- One idea per slide — slides hold the structure, speaker notes hold the depth
- Bullets are prompts for the speaker, not full sentences for the audience to read
- The deck must tell a story: problema → desarrollo → conclusión
- If the consigna specifies a slide count or duration, respect it above all guidelines

## Finding the Consigna and Class Material

The consigna, lineamientos, and class material are usually **PDFs the user already has** — do not ask for them before looking:

1. **Check the conversation** — did the user paste the consigna, notes, or a report?
2. **Search the workspace for the consigna** — Glob `**/*.pdf`, `**/*.docx`, `**/*.md` and Grep for `consigna`, `exposición`, `presentación`, `lineamientos`, `pautas`, `rúbrica`, or the subject name
3. **Search for class material on the topic** — apuntes, PDFs de la cátedra, folders like `apuntes/`, `materias/`, `bibliografia/`. Build slide content **from what the cátedra taught**, with its frameworks and authors — the audience is the professor who taught it
4. **Read what you find** and confirm: "Encontré `[archivo]` — ¿lo uso como base para la presentación?" For large PDFs or several candidates, delegate to the `u-flow:reader` agent — it reads in an isolated context and returns only the relevant extracts
5. **Only if nothing is found**, ask the user or offer `/u-flow:research`

When a consigna/lineamientos exist, extract and honor: cantidad de diapositivas, duración, formato, qué debe incluir (portada, bibliografía, demo), criterios de evaluación. Explicit requirements override every default in this skill.

## Before Generating

Identify:
- **Tema / título** — what is the presentation about?
- **Cantidad de diapositivas** — from the consigna if found; if not given, default to 8-10
- **Duración** — if given, calibrate: roughly 1-2 minutes per content slide
- **Público y contexto** — clase, defensa, coloquio, materia, docente
- **Material disponible** — report, apuntes, class PDFs found in the workspace, or just a topic?

If the user has no topic, ask: "¿Sobre qué tema es la presentación? ¿Cuántas diapositivas y para qué materia?"

If there is no content anywhere and the topic needs research, offer: "¿Querés que investigue el tema primero? Podés usar `/u-flow:research`."

## Plan Mode

Always plan the deck before writing slides — rebuilding a deck with the wrong arc wastes the student's time:
- After discovering the consigna and material, call **EnterPlanMode**
- The plan must show: cantidad de diapositivas, título de cada una, qué cubre cada slide, qué requisito de la consigna responde, duración estimada
- Call **ExitPlanMode** to present the plan; write no slide content until the user approves
- If the user rejects or adjusts (más slides, otro orden, otro foco), revise the plan and present again
- Skip plan mode only for trivial requests: tweaking one slide, rewording bullets, adjusting speaker notes

## Slide Content Format

```markdown
## Presentación: [Título]

### Diapositiva 1 — Portada
**Título:** [Título del trabajo]
**Subtítulo:** [Materia / Docente / Integrantes / Fecha]

### Diapositiva 2 — Introducción
**Título de diapositiva:** Introducción
**Puntos clave:**
- [Contexto en ≤10 palabras]
- [Objetivo de la presentación]
- [Qué se va a ver]
**Nota del orador:** [Lo que se dice al hablar, más completo que el bullet]

### Diapositiva 3 — [Subtema]
**Título de diapositiva:** [Nombre del subtema]
**Puntos clave:**
- [Bullet 1]
- [Bullet 2]
- [Bullet 3]
**Nota del orador:** [Explicación completa para exponer]

### Diapositiva N — Conclusión
**Título de diapositiva:** Conclusión
**Puntos clave:**
- [Síntesis del argumento principal]
- [Respuesta al objetivo]
- [Cierre o implicancia]
**Nota del orador:** [Cierre hablado]

### Diapositiva Final — Preguntas
**Título:** ¿Preguntas?
**Nota del orador:** Agradecer al público y abrir a preguntas.
```

## Standard Deck Structure

For a typical 8-10 slide academic presentation:
1. Portada
2. Introducción (contexto + objetivo)
3-7. Desarrollo (un subtema por diapositiva)
8. Conclusión
9. Preguntas

Scale the desarrollo section up or down to hit the requested slide count.

## Slide Design Principles

- Maximum 5 bullets per slide (3-5 is ideal)
- Each bullet: 8-12 words maximum
- No paragraphs on slides — full explanation lives in the speaker notes
- Speaker notes contain what you *say*, not what's on screen
- Suggest where a diagram or image would strengthen a slide; offer `/u-flow:diagram` to generate one

## Speaker Notes

Speaker notes are the highest-value output for a nervous student. For each content slide write notes the student can actually narrate:
- 3-6 sentences, conversational but precise
- Include the transition into the next slide ("Ahora que vimos X, pasemos a...")
- For oral defenses, anticipate one likely question per key slide and note the answer

## Adapting to Context

- **Defensa / coloquio** — fewer bullets, richer speaker notes; add an "anticipá esta pregunta" cue per slide
- **Presentación grupal** — note suggested speaker handoffs ("acá habla el siguiente integrante")
- **Clase corta (5 min)** — compress to portada + 3 content slides + cierre
- **Exposición de un trabajo ya escrito** — map each report section to one or two slides; do not re-summarize, distill

## Marp Fallback

If the user wants a file that renders to slides without paid tools, offer a Marp markdown version (renders in VS Code with the Marp extension or at marp.app):

```markdown
---
marp: true
theme: default
paginate: true
---

# [Título]
[Materia / Docente / Fecha]

---

## Introducción
- Contexto
- Objetivo
- Estructura

<!-- Nota del orador: lo que se dice al hablar -->

---
```

Each `---` separates a slide; `<!-- comments -->` become speaker notes in Marp presenter mode. Note: speaker notes show in presenter view but do not appear when exporting to PDF or PPTX.

## Pre-Submission Review

When the deck is complete and built from course material, offer a final autonomous review via the `u-flow:reviewer` agent — it verifies slide content against the cátedra's files (no misattributed concepts), consigna requirements (slide count, format, qué debe incluir), and rúbrica coverage. Offer: "¿Reviso la presentación contra el material y la consigna antes de que la expongas?"

## Saving Output

Deliver in chat by default. When the user wants the result saved (or accepts the offer to save it):
- Location: `[materia]/entregas/` — follow the Estructura in the materia's CLAUDE.md if it defines a different layout
- Name: `presentacion-[tema].md`, kebab-case, no accents — e.g. `entregas/presentacion-unidad-3.md`
- If the same TP accumulates 2+ files (informe + presentacion + anexos), group them: `entregas/tp2/informe.md`, `entregas/tp2/presentacion.md` — move the existing loose file in when creating the second
- Create the folder if it doesn't exist; never overwrite an existing file without confirming

## Quality Check

Before finalizing:
- The workspace was searched for consigna/lineamientos and class material before asking
- Slide count and format match the consigna (or the user's ask, or the 8-10 default)
- Content reflects the cátedra's material when it exists — its frameworks, authors, and terminology
- If a rúbrica exists, each evaluation criterion is covered
- Every content slide has both bullets AND speaker notes
- No bullet is a full sentence the audience would just read aloud
- The deck has a clear arc: introducción → desarrollo → conclusión
- A portada and a closing/preguntas slide are present
