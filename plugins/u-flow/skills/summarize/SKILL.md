---
name: u-flow:summarize
description: This skill should be used when the user wants to compress, distill, or restructure academic text into a readable summary for studying or exam prep. Triggers on phrases like "resumir mis apuntes", "haceme un resumen", "resumí esto", "necesito un resumen de", "resumen para el parcial", "apuntes del final", "resumime la unidad", or when the user pastes raw notes and asks for a structured overview to study from. If no content is pasted, looks for course material in the workspace via the reader agent. Produces a readable summary document, not flashcards or questions. Responds in Spanish by default.
allowed-tools:
  - Read
  - Glob
  - Grep
  - Write
---

# Summarize — Academic Notes Summarizer

## Purpose

To transform raw class notes, lecture transcripts, or academic text into structured, exam-ready summaries in Spanish. Apply this skill whenever the user needs to compress content for study purposes.

## Core Principles

- Respond in **Spanish** unless the user explicitly requests another language
- Prioritize clarity over completeness — a good summary removes noise, not meaning
- Structure always beats walls of text — use headings, bullets, and visual hierarchy
- Adapt depth to context: a quick review needs less detail than a parcial summary

## Summary Format

### Standard Summary Structure

Use this structure for general class notes:

```
# Resumen: [Tema / Materia]

## Conceptos Clave
- [Concepto 1]: [definición concisa]
- [Concepto 2]: [definición concisa]

## Ideas Principales
### [Subtema 1]
[2-4 oraciones que capturen la esencia]

### [Subtema 2]
[2-4 oraciones que capturen la esencia]

## Para Recordar
- [Punto crítico 1]
- [Punto crítico 2]
- [Fórmulas o datos exactos si aplica]

## Posibles Preguntas de Examen   ← incluir solo si hay contexto de examen
1. [Pregunta probable]
2. [Pregunta probable]
```

### Exam-Focused Summary

When the user mentions a specific exam or parcial, add:
- "Para el Examen" section highlighting likely questions
- Key definitions the professor emphasized
- Any formulas, dates, or exact data that must be memorized verbatim

## Sourcing the Content

If the user names a topic, materia, or unidad without pasting content ("resumime la unidad 3 de Historia"):
1. **Check the conversation** — was content pasted earlier?
2. **Search the workspace** — delegate to the `u-flow:reader` agent: it finds and reads the course material (PDFs, apuntes) in an isolated context and returns the relevant extracts with file/page citations
3. **Summarize from the extracts**, citing source files so the student can go back to them
4. **Only if nothing exists**, ask for the material or offer `/u-flow:research`

When the summary was built from course material, offer a verification pass via the `u-flow:reviewer` agent — it re-reads the sources and checks fidelity (no altered definitions) and completeness (no missing core concepts). Studying from a wrong summary is worse than not having one. Offer: "¿Verifico el resumen contra tus apuntes antes de que estudies de él?"

## Summarization Process

1. **Read and categorize** — identify main topics, subtopics, and supporting details
2. **Extract key concepts** — pull out definitions, laws, theories, and named concepts
3. **Eliminate redundancy** — remove repeated examples, filler, and tangential content
4. **Restructure** — organize by logical hierarchy, not by the order content appeared in notes
5. **Add exam layer** — if content suggests an exam context, generate probable questions
6. **Calibrate length** — use the word-count tiers in Output Calibration below as the authoritative target; as a rough heuristic, a 10-page reading yields a standard (400-800 word) summary

## Adapting to Content Type

### Science / Technical Content
- Keep formulas exact and visually distinct (use code blocks or bold)
- Use numbered steps for processes and procedures
- Define every acronym or specialized term

### Humanities / Social Sciences
- Capture author/theorist positions in one sentence each
- Note cause-effect relationships explicitly
- Summarize arguments, not just conclusions

### History
- Use chronological or thematic structure depending on content
- Include key dates, actors, and events as bullet points
- Note periodization when relevant

### Mathematics
- Do not paraphrase formulas — copy exactly
- Explain when to apply each formula in plain Spanish
- Include a worked example when the user provides one

## Handling Incomplete Notes

If the user's notes are sparse, disorganized, or clearly missing content:
1. Summarize what is present
2. Flag gaps explicitly: "Nota: este tema parece incompleto — faltan [X]"
3. Offer to search for supplementary information if needed

## Output Calibration

- **Quick review** (user says "rápido" or "básico"): 200-400 words, bullets only
- **Standard summary**: 400-800 words, structured with headings
- **Deep study** (user says "parcial importante" or "final"): 800-1500 words, includes probable questions and key terms section

## Múltiples Fuentes

When the user provides multiple texts (two authors, several chapters, or multiple readings for the same topic):
1. Identify shared concepts and divergences between sources
2. Organize the summary by concept, not by source — integrate the views
3. Use inline attribution: "Según Foucault...", "Bourdieu, en cambio, sostiene..."
4. Add a "Convergencias y Diferencias" section if the sources take distinct positions

## Modo Examen Oral

When the user mentions "examen oral", "coloquio", or "me pregunta el profesor":
- Replace long paragraphs with **frases clave** (2-5 words each) that serve as spoken recall anchors
- Add a "Cómo lo explico en voz alta" section: a sample 3-5 sentence spoken answer for each main concept
- Prioritize definitions and examples the student can narrate fluently, not detailed written arguments

## Saving Output

Deliver in chat by default. When the user wants the result saved (or accepts the offer to save it):
- Location: `[materia]/estudio/` — follow the Estructura in the materia's CLAUDE.md if it defines a different layout
- Name: `resumen-[tema].md`, kebab-case, no accents — e.g. `estudio/resumen-ciclo-economico.md`
- If the same tema accumulates 2+ artifacts (resumen + flashcards + diagrama), group them: `estudio/[tema]/resumen.md`, `estudio/[tema]/flashcards.md` — move the existing loose artifact in when creating the second
- Create the folder if it doesn't exist; never overwrite an existing file without confirming

## Quality Check

Before finalizing the summary:
- Verify every concept the source treats as central appears at least once
- Prefer sentences under ~25 words in the summary body (quotes and oral-mode spoken answers are exempt)
- Verify definitions are precise, not paraphrased vaguely
- Verify the "Para Recordar" section captures the 3-7 most critical facts
- If source material is already condensed (professor slides, class outlines): do not compress further — restructure instead
