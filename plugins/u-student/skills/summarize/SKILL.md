---
name: summarize
description: Activate for quick, single-source summaries when a student pastes content and asks to summarize it on the spot — "resumime esto", "hacé un resumen", "sintetizá". For multi-file, full-subject summaries, use the /u-student:summarize command instead.
---

When asked to summarize academic content on the spot:

## Course context (always check first)

Look for `CLAUDE.md`, `cronograma.md`, and `programa.md` in the current directory.
If found, use them to:
- Identify which class/unit the content belongs to
- Use the course's terminology, not generic definitions
- Detect if the content seems incomplete relative to the cronograma

If a topic is in the cronograma but the provided content seems thin or missing key points:
> ⚠️ Este contenido parece no cubrir todo lo que indica el cronograma para [Clase NN — Tema]. Puede faltar material.

## Summary structure

1. **Tema** — one-line title with class number if detectable
2. **Conceptos clave** — bullet list of essential terms and ideas, using course terminology
3. **Desarrollo** — concise prose summary in logical sections
4. **Preguntas de autoevaluación** — 3–5 questions the student can use to self-test

## Rules

- Adapt length proportionally to input — short text → tight summary, long text → more sections
- Always use the course's own terminology over generic academic language
- Respond in Spanish unless the student specifies otherwise
- Prioritize understanding over completeness — if forced to cut, keep the most exam-relevant content

## When to redirect

If the student wants to summarize:
- Multiple files at once
- An entire partial or subject
- Files they haven't shared yet (PDFs, images, URLs)

→ Tell them: "Para resumir múltiples archivos o toda la materia, usá `/u-student:summarize` que te guía paso a paso con el cronograma."

## After summarizing

Offer next steps:
```
💡 Para seguir estudiando este tema:
  /u-student:quiz     — practicá sin ver las respuestas
  /u-student:mindmap  — visualizá las conexiones entre conceptos
  /u-student:study    — sesión guiada si algo no quedó claro
```
