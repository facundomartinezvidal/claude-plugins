---
name: explain
description: Feynman technique — the student explains a topic in their own words and receives correction and feedback based on course material. Identifies gaps and misconceptions.
argument-hint: "[topic to explain]"
allowed-tools: Read, Bash
---

Run a Feynman technique session: the student explains, the command evaluates against course material and gives targeted feedback.

## Setup

Run workflow-core steps W1, W2, W3.
Show W3 warning only if the topic is in the cronograma but has no material file.

## Step 1 — Set the topic

If the topic is clear from the argument, confirm:
"Bien, explicame [topic] como si se lo explicaras a alguien que no lo conoce. Usá tus propias palabras — no copies definiciones."

If topic is not clear, ask: "¿Qué tema querés practicar explicando?"

## Step 3 — Listen to the student's explanation

Wait for the student to write their explanation in full.
Do not interrupt or correct while they are explaining.

## Step 4 — Evaluate against course material

Compare the student's explanation against the course material. Identify:

**What they got right**: concepts accurately explained, correct relationships, good analogies.
**What they got wrong**: factual errors, inverted cause/effect, wrong terminology.
**What they missed**: key concepts present in the material not mentioned at all.
**Depth**: was the explanation surface-level or did it show real understanding?

## Step 5 — Give structured feedback

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🧠 Feedback — [Topic]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ Bien explicado:
- [accurate point 1]
- [accurate point 2]

⚠️ Imprecisiones:
- Dijiste "[student's words]" → En realidad: [correct version from material]

❌ Faltó mencionar:
- [missing concept 1] — [brief explanation of what it is]
- [missing concept 2]

📊 Evaluación general:
[Superficial / Correcta pero incompleta / Sólida / Excelente]

💡 Intentá de nuevo incluyendo lo que faltó. O preguntame sobre cualquier punto específico.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Step 6 — Iterate

If the student wants to try again, repeat from Step 3.
Track improvement across attempts.

After a solid explanation, suggest:
```
✅ Dominás el tema. Para reforzar con preguntas: /u-student:quiz [topic]
```
