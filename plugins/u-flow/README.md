# u-flow

Academic productivity plugin for university students. Organize your course material, research with your cátedra's sources, write reports and presentations against the real consigna, and generate study aids — all in Spanish (rioplatense), all grounded in *your* course files, not generic internet content.

## Why u-flow

Professors grade against what they taught. u-flow's core design principle is **material-first**: every skill looks at your course files (apuntes, PDFs de cátedra, consignas) before touching the web or generic knowledge, and everything generated can be verified back against those sources.

```
reader  ──►  skills (generate, with you in the loop)  ──►  reviewer
(reads your                                            (verifies output
 course PDFs)                                           against sources)
```

## Quick Start

```bash
/plugin install u-flow@fmartinezvidal-plugins
```

Then, inside your course folder:

```
/u-flow:init          ← set up the materia (offers to organize the folder too)
/u-flow:summarize unidad 3
/u-flow:report TP2    ← finds the consigna PDF by itself
```

## How It Works

### 1. Set up a materia once

`/u-flow:init` scans your folder for the programa, cronograma, and bibliografía (they're usually PDFs you already have), asks one round of questions for the gaps, and writes a `CLAUDE.md`:

```markdown
# Macroeconomía
## Cronograma
| 15/06/2026 | Parcial 1 — unidades 1-3 |
## Programa
### Unidad 1 — Ciclo Económico
- Temas: ... | Material: `01-unidad-1.pdf` | Estado: vista
```

`CLAUDE.md` loads automatically every session — from then on, every skill knows your materia: which unidad covers what, when the parcial is, where the consignas live.

### 2. Keep the folder organized

`/u-flow:organize` turns a messy course folder into:

```
materia/
├── CLAUDE.md
├── programa.pdf · cronograma.pdf      ← course-wide docs, unnumbered
├── content/
│   ├── primer-parcial/
│   │   ├── 01-unidad-1-introduccion.pdf
│   │   └── 02-unidad-2-mercados/      ← 3+ files → folder
│   │       ├── teoria.pdf
│   │       └── practica.pdf
│   └── segundo-parcial/
├── consignas/                         ← consignas, lineamientos, rúbricas
├── estudio/                           ← what u-flow generates for studying
└── entregas/                          ← your own deliverables
```

Files are sequenced by when they were covered in the cursada and renamed consistently (`NN-tipo-descripcion.ext`). Nothing moves without your approval of the full old→new mapping (plan mode). No file is ever lost: count before == count after.

### 3. Generate from your material

Every content skill follows the same source priority:

1. Content you pasted in chat
2. **Your course files** — read via the `reader` agent (isolated context, so big PDFs don't burn your conversation)
3. Web (research) or general knowledge — only for gaps, always flagged

Generated files land in the right place automatically (`estudio/` for study material, `entregas/` for deliverables).

### 4. Verify before submitting or studying

The `reviewer` agent re-reads the original sources and checks the output against them: misattributed claims, invented citations, altered definitions, missing core concepts, consigna/rúbrica compliance. Verdict + findings with draft-vs-source quotes. It never rewrites — you stay in control.

## Commands

| Command | What it does |
|---------|-------------|
| `/u-flow:init` | Set up a materia's CLAUDE.md (cronograma, programa, bibliografía) |
| `/u-flow:organize` | Organize the materia's files by parcial, sequenced and renamed |
| `/u-flow:summarize` | Exam-ready summary from your apuntes or a named unidad |
| `/u-flow:flashcards` | Flashcard deck + interactive quiz mode (`modo quiz`) |
| `/u-flow:diagram` | Excalidraw mind map / flowchart (+ Mermaid fallback) |
| `/u-flow:report` | Academic report — finds and follows the consigna PDF |
| `/u-flow:presentation` | Slide content + speaker notes — respects the consigna's slide count |
| `/u-flow:research` | Sourced research: your material first, SciELO/Redalyc/Dialnet for gaps |

`/u-flow:report` and `/u-flow:presentation` always plan first (plan mode): you approve the structure before anything is written.

## Skills

Skills auto-activate from natural phrases — no command needed:

| Skill | Triggers on things like |
|-------|------------------------|
| `summarize` | "resumime la unidad 3", "resumen para el parcial" |
| `study-aids` | "haceme flashcards", "preguntame sobre", "preparame para el oral" |
| `diagrams` | "mapa mental de", "diagrama de flujo" |
| `documents` | "redactar el TP", "monografía sobre", "corregime la redacción" |
| `presentations` | "armar las diapositivas", "tengo que exponer" |
| `research` | "investigá sobre", "fuentes para la bibliografía" |
| `init` | "nueva materia", "configurá la materia" |
| `organize` | "ordename los archivos", "está todo desordenado" |

## Agents

| Agent | Role |
|-------|------|
| `reader` | Reads course material (PDFs, apuntes) in an isolated context; returns only relevant extracts with file/page citations. Used by every skill that needs course content |
| `reviewer` | Verifies any generated artifact against the original sources + consigna/lineamientos/rúbrica; returns findings by severity. Never rewrites |
| `planner` | Builds a complete study plan autonomously: summary + flashcards + practice questions + mind map + cronograma, calibrated to days until the exam |

## Design Principles

- **Spanish by default** — rioplatense, voseo included
- **Material-first** — the cátedra's framing outranks any web source; conflicts are surfaced, cátedra version leads
- **You approve before anything happens** — plan mode for documents/presentations, full mapping before organize moves files, confirmation before saving
- **Verifiable output** — everything cites its source (file + page); the reviewer can check any artifact against the originals
- **Honest fallbacks** — content from general knowledge is always flagged as such

## Example Session

```
> /u-flow:init
  Encontré programa.pdf y cronograma_1C.pdf — configurando Macroeconomía...
  La carpeta está desordenada — ¿la organizo primero? → sí
  ✓ 23 archivos organizados, CLAUDE.md creado

> tengo el parcial en 5 días, armame un plan
  [planner] Plan: resumen unidades 1-3 + 25 flashcards + preguntas + mapa mental + cronograma de 5 días

> /u-flow:report TP2
  Encontré consignas/tp2/consigna.pdf — ¿es la consigna? → sí
  [plan mode] Estructura propuesta: 4 secciones mapeadas a los 4 puntos de la consigna → aprobado
  ... escribe sección por sección ...
  ¿Hago una revisión final contra el material y la consigna? → sí
  [reviewer] NECESITA CAMBIOS: sección 2 atribuye a Keynes algo que unidad-3.pdf (pág. 12) contradice
```

## License

MIT
