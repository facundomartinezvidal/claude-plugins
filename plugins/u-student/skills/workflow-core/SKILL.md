---
name: workflow-core
description: Internal workflow foundation used by all u-student commands. Defines the standard setup steps — load subject context, read provided content, detect plan mode, check for missing material. Commands reference this skill instead of repeating boilerplate.
allowed-tools: Read, WebFetch, Bash
---

This skill defines the standard steps that every u-student command runs before its main logic. It is internal — do not explain these steps to the student unless they ask.

---

## STEP W1 — Load subject context

Read the following files from the current directory if they exist:

```bash
ls CLAUDE.md cronograma.md programa.md progreso.md 2>/dev/null
```

- **`CLAUDE.md`** — subject name, professor, career, folder structure, instructions. This is the primary context source. If found, all subsequent steps use its data.
- **`cronograma.md`** — class schedule: maps class numbers to topics and parciales. Used for coverage checks and topic lookup.
- **`programa.md`** — official syllabus: units, depth expectations, bibliography.
- **`progreso.md`** — student's saved study progress from a previous session. Load if found and factor it into any progress-related output.

If **no context files exist** in the current directory:
> ℹ️ No encontré la configuración de la materia en esta carpeta. Para mejores resultados, corré `/u-student:init` en la carpeta de tu materia. Continúo igual con lo que tengas disponible.

Proceed regardless — commands work without context files, just with less accuracy.

---

## STEP W2 — Read provided content

Use the **content-reader** skill to process all user-provided inputs:
- Text pasted in the conversation
- File paths referenced in the argument
- Images (slides, notes, whiteboards)
- URLs
- Transcriptions

content-reader returns normalized content blocks tagged with `[SOURCE]`, `[CLASS]`, `[PARTIAL]`, and a `[COVERAGE]` summary.

If **no content provided** and the command requires it, ask:
> "¿Qué contenido querés usar? Podés pegar texto, compartir archivos, imágenes o URLs."

---

## STEP W3 — Check for missing material

Using the `[COVERAGE]` summary from W2 and the `cronograma.md` loaded in W1:

1. Identify classes in the cronograma that have **no material file** in `content/`
2. Identify classes the student **did not provide** in this session

Build a warning list:
```
⚠️ Sin material para estas clases del cronograma:
  - Clase [NN] — [Tema]
  - Clase [NN] — [Tema]
```

Pass this list to the command. The command decides when and how to show it to the student (before plan mode, after, or inline during output).

If cronograma is not available → skip this step silently.

---

## STEP W4 — Detect plan mode

Run:
```bash
git log --oneline -1 2>/dev/null
```

- **Output is non-empty** (repo has commits) → set `PLAN_MODE = ultraplan` → use `/ultraplan` for the planning step
- **Empty output or error** → set `PLAN_MODE = planmode` → use `EnterPlanMode` tool

Store this value. The command uses it in its plan step.

If the command does not use a plan step (e.g., export, organize), skip W4.

---

## STEP W5 — Language rule (always active)

Respond in **Spanish** in all student-facing output.

Exceptions:
- Student writes in English → respond in English
- Source material is in English and the student hasn't specified a language preference → summarize/explain in Spanish, quote source material in its original language

This rule is always active across all commands and agents. It does not need to be stated in the output.

---

## Usage pattern in commands

Commands reference these steps like this:

```
## Setup
Run workflow-core steps W1, W2, W3, W4.
Use PLAN_MODE value from W4 for the plan step below.
```

Or selectively:
```
## Setup
Run workflow-core steps W1 and W3 (no content input needed, no plan step).
```

Commands that do NOT use workflow-core:
- `init` — creates the context files, so context doesn't exist yet
- `export` — file conversion only, no course context needed
