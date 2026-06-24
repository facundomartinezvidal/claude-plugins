---
name: u-flow:presentation
description: Create presentation slide content and speaker notes for university in Spanish
argument-hint: "[tema] [número de diapositivas opcional]"
allowed-tools:
  - Read
  - Glob
  - Grep
  - EnterPlanMode
  - ExitPlanMode
---

Load the u-flow:presentations skill and generate presentation slide content.

The user has invoked `/u-flow:presentation` with the following arguments: 

## Instructions

1. Parse arguments for:
   - Topic or title
   - Number of slides (if specified); default to 8-10 slides
   - Any consigna or context about the audience or course
2. **Search the workspace before asking** (per the presentations skill):
   - Consigna/lineamientos: Glob `**/*.pdf`, `**/*.docx`, `**/*.md`; Grep for `consigna`, `exposición`, `lineamientos`, `pautas`, `rúbrica`
   - Class material on the topic: apuntes, PDFs de la cátedra, folders like `apuntes/`, `materias/`
   - If found, confirm: "Encontré `[archivo]` — ¿lo uso como base?"
3. If no topic and nothing found, ask: "¿Sobre qué tema es la presentación? ¿Cuántas diapositivas necesitás y para qué materia?"
4. Enter plan mode (EnterPlanMode) and build the deck plan: cantidad de diapositivas, título y contenido de cada una, qué requisito de la consigna responde. Present via ExitPlanMode and wait for approval
5. Generate slide-by-slide content following the u-flow:presentations skill format:
   - Slide number and title
   - 3-5 bullet points (8-12 words each)
   - Speaker notes with full explanation of what to say
   - Built from the cátedra's material when it exists
6. Include: cover slide, intro, 5 content slides, conclusion, questions slide (scale content slides to hit the requested count; consigna requirements override)
7. At the end, ask: "¿Querés que adapte el contenido para un público específico o que cambie la cantidad de diapositivas?"

## Output

Markdown with one section per slide. Include speaker notes. Always respond in Spanish.
