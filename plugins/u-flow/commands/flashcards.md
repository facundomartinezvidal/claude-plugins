---
name: u-flow:flashcards
description: Generate a flashcard deck from academic content for exam preparation in Spanish
argument-hint: "[tema o materia] — pegá el contenido a estudiar"
allowed-tools:
  - Read
  - Glob
  - Grep
---

Load the u-flow:study-aids skill and generate flashcards from the user's content.

The user has invoked `/u-flow:flashcards` with the following arguments: 

## Instructions

1. If arguments include content, generate flashcards immediately
2. If only a topic name is provided, **search the workspace first** via the u-flow:reader agent — it reads course PDFs/apuntes in isolation and returns relevant extracts. Build cards from the cátedra's material. Only if nothing is found, ask the user to paste notes
3. If no arguments, ask: "¿Sobre qué tema querés las flashcards? Pegá tus apuntes o decime los conceptos clave."
4. Apply the study-aids skill: numbered list format (question bold, answer as blockquote)
5. Scale quantity per the skill's Output Calibration: 5-10 quick review, 11-20 standard, 21-30 exam prep
6. After generating, ask: "¿Querés que te haga las preguntas en modo quiz interactivo?"

## Output

Numbered flashcard list per the study-aids skill format. Always respond in Spanish.
