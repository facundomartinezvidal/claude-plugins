---
name: quiz
description: Interactive quiz from course material — asks questions one by one without showing the answer, then gives detailed feedback on the student's response. Always prioritizes course material.
argument-hint: "[topic, files, or URLs] [--count N] [--parcial 1|2]"
allowed-tools: Read, WebFetch, Bash
---

Run an interactive quiz based on course material. Never show the answer before the student responds.

## Setup

Run workflow-core steps W1, W2, W3.
If `--parcial 1` or `--parcial 2` provided, scope W2 to that partial only.
Show W3 missing-content warnings before generating questions.

## Step 1 — Generate questions

Create N questions (default 10, override with `--count N`) from course material.

Question types — mix across the session:
- **Definición**: "¿Qué es [concepto]?"
- **Relación**: "¿Cuál es la diferencia entre [A] y [B]?"
- **Aplicación**: "¿En qué situación usarías [concepto/método]?"
- **Proceso**: "¿Cuáles son los pasos para [procedimiento]?"
- **Causa/efecto**: "¿Qué pasa cuando [condición]?"

Only ask about content present in the course material.

## Step 3 — Run quiz

For each question, show only the question — never the answer:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
❓ Pregunta [N]/[Total]
[Clase NN — Tema if from cronograma]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Question]

Tu respuesta:
```

Wait for student answer. Then evaluate:

```
[✅ / ⚠️ / ❌] [Correct / Partially correct / Incorrect]

📖 Respuesta según el material:
[Answer based on course content — cite class/file if possible]

[If partially correct or wrong:]
💡 Lo que faltó o estuvo impreciso: [specific gap]
```

Ask: "¿Continuamos? (s / repetir esta / saltar)"

## Step 4 — Final report

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Resultado del quiz

✅ Correctas:   [N]
⚠️ Parciales:  [N]
❌ Incorrectas: [N]
Puntaje: [X]%
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Temas para repasar:
[list topics where student struggled]

💡 Para estudiar esos temas: /u-student:study [topic]
```
