---
name: u-flow:study-aids
description: This skill should be used when the user wants to generate flashcards, practice questions, or Q&A decks from academic content, wants to be quizzed interactively, or wants to prepare for an oral exam. Triggers on phrases like "haceme flashcards", "preguntas de práctica", "quiz sobre esto", "tarjetas de estudio", "preguntame sobre", "modo quiz", "quiero repasar con preguntas", "generame un cuestionario", "hacé de profesor y preguntame", "preparame para el oral", "examen oral", "coloquio", "tengo el parcial/final y quiero practicar", or when the user wants to test their knowledge on a topic. If no content is pasted, looks for course material in the workspace via the reader agent. Responds in Spanish by default.
allowed-tools:
  - Read
  - Glob
  - Grep
  - Write
---

# Study Aids — Flashcards and Practice Q&A

## Purpose

To generate structured study aids — flashcards and practice questions — from academic content. Apply when the user wants to test recall, prepare for oral or written exams, or self-quiz on a topic.

## Core Principles

- Respond in **Spanish** unless the user explicitly requests another language
- Each flashcard tests exactly one concept — no compound questions
- Practice questions should mirror real exam difficulty and style
- Vary question types: definition, application, comparison, analysis

## Flashcard Format

Output flashcards as a numbered list (renders everywhere, including mobile):

```markdown
## Flashcards: [Tema]

**1.** ¿Qué es [concepto]?
> [definición precisa en 1-2 oraciones]

**2.** ¿Cuál es la diferencia entre X e Y?
> [distinción clave]

**3.** ¿Cuándo se aplica [ley/fórmula]?
> [contexto de aplicación]

**4.** ¿Quién propuso [teoría] y cuándo?
> [autor + año]
```

If the user explicitly asks for a table format, use the Markdown table instead.

### Flashcard Quality Rules

- Front: always a question, never a statement
- Back: complete answer that makes sense without the front
- Maximum 3 sentences per answer — if more is needed, split into two cards
- Technical terms on front, full explanation on back
- For formulas: formula on front, name + variables + when to use on back

## Practice Q&A Format

Generate practice questions in three difficulty tiers:

```markdown
## Práctica: [Tema]

### Nivel 1 — Definición y Recall
1. ¿Qué significa [término]?
2. ¿Cuáles son las características de [concepto]?

### Nivel 2 — Comprensión y Aplicación
3. Explicá con tus palabras cómo funciona [proceso].
4. Dado [escenario], ¿qué [ley/método/herramienta] aplicarías y por qué?

### Nivel 3 — Análisis y Síntesis
5. Compará [X] con [Y] en términos de [criterio].
6. ¿Cuáles son las limitaciones de [teoría/modelo]?

---
**Respuestas sugeridas:**

1. [Respuesta concisa]
2. [Respuesta concisa]
...
```

## Study Aid Generation Process

0. **If no content is provided** — for a named topic with no pasted notes ("preguntame sobre la Revolución Francesa"): first search the workspace via the `u-flow:reader` agent (it reads course PDFs/apuntes in isolation and returns relevant extracts); build cards from the cátedra's material when it exists. If nothing is found and the topic is general, generate from your own knowledge; if it is narrow or institution-specific, ask the user for their material
1. **Extract testable content** — identify definitions, processes, relationships, dates, formulas, authors
2. **Categorize by type** — factual recall vs. comprehension vs. analysis
3. **Generate questions first**, then answers — ensures questions are self-contained
4. **Scale quantity** to content: ~1 flashcard per key concept, then bound the total to the band in Output Calibration
5. **Add difficulty markers** if the user asks for exam prep

## Adapting to Subject

### Sciences / Engineering
- Include formula recognition cards (formula → what it calculates)
- Add units and common mistakes as card backs
- Pair process cards with step-by-step answers

### Humanities / Social Sciences
- Focus on theorists, dates, arguments, and critiques
- Include "¿Estás de acuerdo? ¿Por qué?" prompts for deeper thinking
- Create comparison cards for rival theories or approaches

### History
- Use event → consequence pairs
- Create timeline ordering questions
- Include cause-effect cards

### Languages
- Vocabulary: term on front, definition + example sentence on back
- Grammar rules: rule on front, 2 examples on back

## Interactive Mode

If the user says "preguntame", "modo quiz", or "hacé de profesor", enter interactive quiz mode:
1. Present one question at a time
2. Wait for the user's answer
3. Provide feedback: correct/incorrect + the full correct answer
4. Track score at the end: "Resultado: X/Y respuestas correctas"
5. Offer to review incorrect answers
6. Exit when the user says "parar", "basta", "listo", or "salir" — show final score immediately

## Verification Pass

When the deck was built from course material (via reader or workspace files), offer a review via the `u-flow:reviewer` agent — it re-reads the sources and verifies each card's answer against what the cátedra actually says, and that no central concept is missing. Offer: "¿Verifico las tarjetas contra el material antes de que practiques?"

## Modo Examen Oral

When the user mentions "examen oral", "coloquio", or "me toman de palabra":
- Replace written Q&A with dialogue-style prompts: "El profesor te pregunta: [pregunta]. ¿Qué respondés?"
- Format answers as spoken phrases (1-3 sentences), not written paragraphs
- Add a "Conceptos que hay que poder explicar sin leer" section with the 5-8 most critical terms
- If the user wants to practice live, reuse the Interactive Mode loop (one prompt at a time, feedback, score, exit on "parar"/"basta"/"salir")

## Output Calibration

- **Quick review** (5-10 cards): for topics the user already knows partially
- **Standard deck** (11-20 cards): default for a lecture or chapter
- **Full exam prep** (21-30 cards + practice questions): when user mentions parcial or final
- **Interactive quiz**: default 8-10 questions per session unless the user specifies otherwise

## Saving Output

Deliver in chat by default. When the user wants the result saved (or accepts the offer to save it):
- Location: `[materia]/estudio/` — follow the Estructura in the materia's CLAUDE.md if it defines a different layout
- Name: `flashcards-[tema].md`, kebab-case, no accents — e.g. `estudio/flashcards-unidad-2.md`
- If the same tema accumulates 2+ artifacts (resumen + flashcards + diagrama), group them: `estudio/[tema]/resumen.md`, `estudio/[tema]/flashcards.md` — move the existing loose artifact in when creating the second
- Create the folder if it doesn't exist; never overwrite an existing file without confirming

## Quality Check

Before finalizing:
- Every flashcard has a question-form front
- No answer requires reading another card to make sense
- Practice questions include at least one analysis-level item
- Answers are provided separately from questions (so user can self-test)
