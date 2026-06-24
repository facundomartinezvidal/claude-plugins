---
name: flashcards
description: Generate study flashcards from course material — organized by class and partial exam period. Shows question first, waits for student, then reveals answer. Always prioritizes course material.
argument-hint: "[topic, files, or URLs] [--count N] [--parcial 1|2]"
allowed-tools: Read, WebFetch, Bash
---

Generate and run flashcard study sessions from course material.

## Setup

Run workflow-core steps W1, W2, W3.
If `--parcial 1` or `--parcial 2` flag provided, scope W2 content loading to that partial only.
Show W3 missing-content warnings before generating cards.

## Step 1 — Generate flashcards

Extract key concepts, definitions, relationships, formulas, and examples from the course material.

Default: 10 cards. Override with `--count N`.

Prioritize:
1. Definitions and key terms from the material
2. Relationships between concepts
3. Formulas or processes
4. Examples the professor used in class (if in material)

Do not use generic textbook definitions if the course material defines the concept differently — always use the course's version.

## Step 3 — Run flashcard session

For each card, show only the question:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📇 Flashcard [N]/[Total]
[Clase NN — Tema if from cronograma]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

❓ [Question]

Respondé cuando estés listo (o escribí "ver" para ver la respuesta).
```

Wait for student response, then reveal:

```
✅ Respuesta:
[Answer from course material]

[Source: Clase NN — filename if applicable]
```

Ask: "¿La sabías? (s/n)" — track score silently.

Move to next card.

## Step 4 — Session summary

After all cards:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Resultado: [N]/[Total] correctas

✅ Sabías: [list topics]
❌ Repasar: [list topics]

💡 Para los temas que fallaste:
  /u-student:quiz [topic]    — preguntas interactivas con feedback
  /u-student:study [topic]   — sesión guiada para entenderlo
  (tutor agent)              — si necesitás que alguien te explique
  (progress-tracker agent)   — para ver tu cobertura total de la materia
```
