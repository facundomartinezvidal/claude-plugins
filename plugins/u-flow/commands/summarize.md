---
name: u-flow:summarize
description: Summarize class notes or academic text into an exam-ready study summary in Spanish
argument-hint: "[materia o tema] — pegá tus apuntes a continuación"
allowed-tools:
  - Read
  - Glob
  - Grep
---

Load the u-flow:summarize skill and apply it to the user's content.

The user has invoked `/u-flow:summarize` with the following arguments: 

## Instructions

1. If arguments include notes or text, begin summarizing immediately
2. If arguments include only a subject or unidad name (e.g., "Física", "unidad 3 de Historia"), **search the workspace first** via the u-flow:reader agent — it reads course PDFs/apuntes in isolation and returns relevant extracts. Summarize from those, citing files. Only if nothing is found, ask the user to paste the notes
3. If no arguments are provided, ask: "¿Sobre qué tema querés el resumen? Pegá tus apuntes o describí el contenido."
4. Apply the summarize skill: extract key concepts, restructure by logical hierarchy, and produce an exam-ready summary in Spanish
5. At the end, offer: "¿Querés que también genere flashcards o un mapa mental sobre este tema?"

## Output

Produce the summary in Markdown with clear headings. Always respond in Spanish.
