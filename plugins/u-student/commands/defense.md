---
name: defense
description: Prepare or simulate an oral defense of a presentation — either generates a written defense plan or runs an interactive slide-by-slide simulation with feedback
argument-hint: "[presentation.html or topic] [--plan | --simulate]"
allowed-tools: Read, EnterPlanMode, Bash
---

Help a student prepare or simulate the oral defense of their presentation.

## Setup

Run workflow-core step W1 only (load subject context for subject name and metadata).
Then load the presentation: if an HTML file is provided, read it to extract slide titles and content.
If no file, ask: "¿Tenés la presentación generada? Compartila o decime el tema y los puntos que vas a cubrir."

## Step 1 — Determine mode

If `--plan` flag provided → go to Plan mode.
If `--simulate` flag provided → go to Simulate mode.
If neither, ask:

```
¿Qué querés hacer?

1. 📋 Plan de defensa — te armo una guía escrita con qué decir en cada slide, cómo manejar el tiempo y preguntas probables del profesor
2. 🎤 Simulación — exponés slide por slide y te doy feedback en tiempo real sobre contenido, tiempo y claridad
```

---

## PLAN MODE

Generate a written defense guide with this structure:

### Guía de defensa — [Topic]

**Tiempo total:** [N] minutos

**Por slide:**

| # | Título | Qué decir (puntos clave) | Tiempo sugerido |
|---|--------|--------------------------|-----------------|
| 1 | Título | Presentarte, nombrar el tema, mencionar la materia | 30 seg |
| 2 | Índice | Recorrer brevemente los temas a cubrir | 30 seg |
| … | … | … | … |
| N | Conclusión | Sintetizar los 3 puntos más importantes, cerrar con una reflexión | 1 min |

**Frases de transición entre slides:**
- "A partir de esto, pasamos a…"
- "Esto nos lleva al siguiente punto…"
- "Como vimos, [X], lo cual es importante porque…"

**Preguntas probables del profesor:**
[Based on the content, generate 5–8 questions the professor might ask, with suggested answers]

| Pregunta | Cómo responder |
|----------|---------------|
| ¿Por qué elegiste este enfoque? | … |
| ¿Qué limitaciones tiene lo que presentás? | … |

**Consejos finales:**
- Si no sabés algo: "No tengo esa información ahora, pero lo puedo investigar"
- Si te trabás: volvé al slide actual y retomá el bullet
- Mirá al profesor cuando hacés las transiciones entre slides

---

## SIMULATE MODE

Run an interactive slide-by-slide simulation.

Tell the student:
```
🎤 Simulación de defensa — [Topic]

Voy a ir mostrándote cada slide uno por uno.
Vos exponés como si estuvieras frente al profesor — escribí lo que dirías en voz alta.
Yo te doy feedback después de cada slide sobre:
- ✅ Qué estuvo bien
- ⚠️ Qué faltó o fue confuso
- ⏱ Tiempo estimado (basado en lo que escribiste)

¿Listo? Empezamos con el Slide 1.
```

For each slide:
1. Show the slide title and bullets
2. Wait for student to "present" it in writing
3. Give structured feedback:

```
**Feedback — Slide [N]:**
✅ Bien: [what they covered correctly]
⚠️ Faltó: [key point from the slide they missed]
💡 Sugerencia: [how to say it better]
⏱ Tiempo: ~[N] segundos — [fast/on track/slow]
```

4. Move to next slide

After all slides, give a final summary:

```
## Resumen de la simulación

⏱ Tiempo total estimado: [N] minutos [vs target time]
✅ Puntos fuertes: [what they consistently did well]
⚠️ A mejorar: [recurring issues]
🎯 Recomendación: [1-2 specific things to practice before the real defense]
```
