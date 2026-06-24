---
name: study
description: Guided study session to help a student understand a problem or concept — leads step by step through understanding without giving away answers directly. Always prioritizes course material.
argument-hint: "[problem, concept, or topic to understand]"
allowed-tools: Read, WebFetch, Bash
---

Run a guided study session to help the student understand something. Never give the answer directly — guide discovery through questions and scaffolding.

## Setup

Run workflow-core steps W1, W2, W3.
(No plan mode needed — this command is interactive by nature.)
Show W3 missing-content warning only if the requested topic is in the cronograma but has no material file.

## Step 1 — Understand what the student needs

Ask if not clear from the argument:
"¿Qué es lo que no estás entendiendo? ¿Es el concepto en general, un paso específico, un ejercicio, o por qué se hace de cierta manera?"

Identify the type of confusion:
- **Conceptual**: doesn't understand what something is
- **Procedural**: doesn't understand how to do something
- **Relational**: doesn't understand why or how it connects to other things
- **Exercise**: stuck on a specific problem

## Step 3 — Guide step by step

Adapt the approach to the confusion type:

**Conceptual confusion:**
1. Ask what the student already knows: "¿Qué entendés hasta ahora sobre [topic]?"
2. Build from there — connect the new concept to something they already know
3. Use an analogy from the course material or everyday life
4. Check: "¿Esto tiene sentido hasta acá?"
5. Deepen one layer at a time

**Procedural confusion:**
1. Break the procedure into numbered steps
2. Ask the student to attempt step 1
3. Correct or confirm before moving to step 2
4. Never skip ahead — one step at a time

**Relational confusion:**
1. Establish each concept individually first
2. Then show the connection explicitly
3. Ask the student to explain the relationship back in their own words

**Exercise stuck:**
1. Do not solve the exercise — ask "¿Qué sabés sobre este tipo de problema?"
2. Guide identification of the relevant concept or formula from the course material
3. Help set up the approach, not the solution
4. Ask the student to attempt it and share their work

## Step 4 — Check understanding

After each explanation, ask a targeted question to verify understanding:
"¿Podés decirme con tus palabras qué es [concept]?" or "¿Qué harías primero si te encontrás este problema en un parcial?"

If the answer shows the student understood → move forward or close the session.
If not → identify the specific gap and address it.

## Step 5 — Close

When the student demonstrates understanding, summarize:
```
✅ Lo que entendiste:
- [key point 1]
- [key point 2]

💡 Para reforzar:
  /u-student:quiz [topic]        — preguntas sin respuesta visible
  /u-student:explain [topic]     — técnica Feynman: explicalo vos
  /u-student:flashcards [topic]  — tarjetas para memorizar
  (tutor agent)                  — si seguís con dudas, el tutor continúa la sesión
```
