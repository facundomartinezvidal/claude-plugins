---
name: exam
description: Simulate a full partial exam based on course material and cronograma — mixed question types, optional time limit, graded at the end. Always prioritizes course material and warns about missing content.
argument-hint: "[--parcial 1|2] [--tiempo N] [--preguntas N]"
allowed-tools: Read, Bash
---

Simulate a partial exam based on the student's course material.

## Setup

Run workflow-core steps W1, W2, W3.
Determine parcial scope before W2: if `--parcial 1` → load only primer-parcial; if `--parcial 2` → only segundo-parcial; if neither → ask first.
Show W3 missing-content warnings before starting the exam.

## Step 1 — Configure exam

Ask or apply flags:
- Time limit: `--tiempo N` minutes (default: no limit, but track time)
- Question count: `--preguntas N` (default: 10)

Tell the student:
```
📝 Simulacro — [Parcial] — [Subject if detectable]
Preguntas: [N]
Tiempo: [N minutos / sin límite]
Temas cubiertos: [list from cronograma]

Reglas:
- Respondé cada pregunta antes de ver la siguiente
- Podés escribir "no sé" si no sabés
- Al final recibís tu nota y feedback detallado

¿Empezamos?
```

Wait for confirmation.

If time limit set, note start time: `date +%s`

## Step 3 — Run exam

Show ALL questions upfront numbered — no answers, no hints:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 EXAMEN — [N] preguntas
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. [Question]
2. [Question]
…
N. [Question]

Respondé una por una. Empezá con la 1.
```

For each student answer:
- Acknowledge receipt: "Respuesta [N] registrada. Seguí con la [N+1]."
- Do NOT give feedback yet — wait until all questions are answered

If time limit set and student is taking too long, warn at 80% of time elapsed.

## Step 4 — Grade and feedback

After all answers received, calculate elapsed time if applicable.

For each question evaluate against course material:

Score: ✅ correct (1 pt) / ⚠️ partial (0.5 pt) / ❌ wrong (0 pt)

Final report:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 RESULTADO DEL EXAMEN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Puntaje: [X]/[N] ([%])
Nota: [4–10 scale: <50%=4, 50-59%=5, 60-69%=6, 70-79%=7, 80-89%=8, 90-94%=9, 95-100%=10]
Tiempo: [elapsed if applicable]

CORRECCIÓN DETALLADA:

1. [Question]
   Tu respuesta: [student answer]
   [✅/⚠️/❌] Respuesta correcta: [from course material]
   [If wrong/partial:] Lo que faltó: [specific gap]

…

TEMAS A REPASAR:
[grouped by class/unit]

💡 Para estudiar esos temas: /u-student:study [topic]
💡 Para seguir practicando: /u-student:quiz --parcial [1|2]
💡 Para ver tu cobertura total: (progress-tracker agent)
💡 Para analizar parciales reales: (exam-analyst agent)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
