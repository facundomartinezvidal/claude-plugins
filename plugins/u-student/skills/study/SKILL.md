---
name: study
description: Activate for quick concept lookups and brief explanations when a student asks "qué es X", "definición de", "explicame brevemente" in passing — lightweight, single-concept responses. For deep, interactive understanding sessions, the tutor agent handles the full workflow. For guided step-by-step problem solving, use /u-student:study command.
---

When a student asks a quick conceptual question or needs a brief explanation:

## Course context (always check first)

Look for `CLAUDE.md`, `cronograma.md`, and `programa.md` in the current directory.

If found:
- Find the concept in the cronograma → note the class and unit
- Use definitions and terminology from the course material, not generic ones
- Check if a material file exists for that class in `content/`

If concept is in cronograma but no material file:
> ⚠️ No encontré el material de [Clase NN — Tema]. Explico con conocimiento general — puede diferir de cómo lo ve tu profesor.

If concept is not in cronograma at all:
> ℹ️ Este tema no aparece en el programa de tu materia. ¿Es para esta materia o para otra?

## Quick explanation structure

1. **Qué es**: one clear sentence using course terminology
2. **Ejemplo concreto**: one real example from the subject area
3. **Analogía** (if abstract): one familiar comparison
4. **Conexión**: how it relates to another concept from the same unit

Keep it short — this skill is for quick lookups, not deep sessions.

## When to escalate

If the student:
- Expresses deep confusion ("no entiendo nada", "no me queda claro por qué")
- Is stuck on an exercise
- Needs step-by-step procedural guidance
- Asks follow-up questions after the explanation

→ Hand off to tutor agent: "Para trabajarlo en detalle, el agente tutor puede guiarte paso a paso."

If the student wants a structured study session on a topic:
→ "/u-student:study [topic] — sesión guiada completa"

## After explaining

Always end with one targeted check question:
"¿Podés decirme con tus palabras qué es [concept]?" or "¿Entendiste la diferencia entre X e Y?"

Offer:
```
💡 Para profundizar:
  /u-student:study [topic]   — sesión guiada paso a paso
  /u-student:quiz [topic]    — practicá sin ver las respuestas
```
