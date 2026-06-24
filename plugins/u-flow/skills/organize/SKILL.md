---
name: u-flow:organize
description: This skill should be used when the user wants to organize a materia's files into a clean structure: a content folder split by parcial, files ordered by when they were covered in the cursada, and renamed with consistent nomenclature. Triggers on phrases like "organizá la materia", "ordename los archivos", "organizá el contenido", "está todo desordenado", "ordená la carpeta de la materia", "renombrá los archivos", or when the user's course folder is a mess of inconsistently named files. Updates the materia's CLAUDE.md after organizing (creates it via u-flow:init if missing). Responds in Spanish by default.
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash
  - Write
  - Edit
  - EnterPlanMode
  - ExitPlanMode
---

# Organize — Course Folder Organization

## Purpose

To turn a messy materia folder into a clean, ordered structure: content split by parcial, files ordered by when they were covered, and named consistently. After organizing, the materia's `CLAUDE.md` is updated to reflect the new layout — or created via the `u-flow:init` skill if it doesn't exist.

## Core Principles

- Respond in **Spanish** unless the user explicitly requests another language
- **Never move or rename a file without showing the full plan and getting approval** — these are the student's files
- Order reflects the cursada: when each tema was seen, not alphabetical
- Copy nothing, move everything — no duplicates left behind
- `CLAUDE.md` must end up consistent with the real folder structure

## Target Structure

```
[materia]/
├── CLAUDE.md
├── programa.pdf                  ← course-wide docs live at the root,
├── cronograma.pdf                   unnumbered — they span the whole cursada
├── content/
│   ├── primer-parcial/
│   │   ├── 01-unidad-1-introduccion.pdf
│   │   ├── 02-clase-02-oferta-y-demanda.pdf
│   │   └── ...
│   └── segundo-parcial/          ← only if the materia has one
│       ├── 01-unidad-4-....pdf
│       └── ...
├── consignas/
│   ├── consigna-tp1.pdf
│   └── lineamientos-entrega.pdf
├── estudio/                      ← u-flow-generated study material
│   ├── resumen-unidad-1.md
│   ├── flashcards-unidad-1.md
│   └── diagrama-ciclo-economico.excalidraw
└── entregas/                     ← the student's own work and deliverables
    └── tp1-informe.md
```

- **Programa, cronograma, bibliografía general** → materia root, unnumbered: they belong to the whole cursada, not to a parcial
- No segundo parcial → everything under `content/` directly (still numbered)
- Consignas/lineamientos/rúbricas go to `consignas/`, never mixed with material
- Generated study material (resúmenes, flashcards, diagramas, investigaciones) goes to `estudio/`
- The student's own drafts and submissions go to `entregas/`

## Naming Convention

`NN-tipo-descripcion.ext`

- `NN` — two-digit order of when it was covered (01, 02, ...)
- `tipo` — `unidad`, `clase`, `practica`
- `descripcion` — short kebab-case topic, no accents, no spaces, no uppercase: `oferta-y-demanda`, not `Oferta y Demanda (FINAL) v2`
- Examples: `04-unidad-2-mercados.pdf`, `07-practica-ejercicios-unidad-2.pdf`
- **Unnumbered** (not sequenced content): `programa.pdf`, `cronograma.pdf`, `bibliografia.pdf` at the root; `consigna-tp1.pdf`, `lineamientos-entrega.pdf` in `consignas/`

## Grouping — When a Folder Beats Loose Files

When the same contenido (unidad, tema, or TP) has **3 or more files**, group them in a subfolder instead of prefixing each file:

```
content/primer-parcial/
  01-unidad-1-introduccion.pdf          ← 1-2 files: stays loose
  02-unidad-2-mercados/                 ← 3+ files: folder carries the order number
    teoria.pdf
    practica.pdf
    anexo-casos.pdf
    clase-04-grabada.md
```

- The folder takes the `NN-` prefix and the unidad name; files inside drop the redundant prefix and are named by what they are (`teoria.pdf`, not `02-unidad-2-mercados-teoria.pdf`)
- Same rule for `consignas/` when a TP has consigna + lineamientos + rúbrica + anexos: `consignas/tp2/`
- 1-2 files → keep loose; a folder with one file is noise
- Decide grouping in the plan and show it in the mapping — re-runs may convert a loose file into a folder when a unidad grows past the threshold (show the conversion in the plan)

## Process

### 1. Discover

Delegate bulk reading to the `u-flow:reader` agent:
- Inventory every file: Glob `**/*` in the materia folder
- Find the cronograma/programa — they define **which unidades fall in which parcial** and the order temas were seen
- If a `CLAUDE.md` exists, its Programa section is the primary source for unidad order and parcial boundaries
- For ambiguous files (`final2 (1).pdf`, `scan_034.pdf`), have the reader open them and identify what they contain

### 2. Classify each file

- **Parcial**: which parcial covers it (from cronograma: "parcial 1 — unidades 1-3")
- **Order**: position in the cursada (unidad number, clase number, or cronograma date)
- **Tipo**: material de clase / práctica / consigna / entrega del estudiante / administrativo
- Unclassifiable files → ask the user in ONE batch: "No pude clasificar estos: [lista] — ¿qué son?"

### 3. Plan in plan mode

Call **EnterPlanMode** and build the complete mapping:

```
[raíz]/
  programa.pdf                         ← programa macro 2026.pdf
  cronograma.pdf                       ← cronograma_1C(3).pdf
content/primer-parcial/
  01-unidad-1-introduccion.pdf         ← Unidad 1 (1).pdf
  02-clase-02-oferta-y-demanda.pdf     ← clase 2 oferta.pdf
consignas/
  consigna-tp1.pdf                     ← TP1_consigna_final(2).pdf
SIN CLASIFICAR (no se mueven):
  - foto-pizarron.jpg
```

Every file appears: destination or explicit "no se mueve". Call **ExitPlanMode** — move nothing until approved.

### 4. Execute

- `mkdir -p` the structure, then `mv` per the approved mapping (Bash)
- Move, never copy
- If a destination name collides, suffix `-b` and flag it as a probable duplicate for the user to review

### 5. Update CLAUDE.md

- **Exists** → Edit it: update the Estructura section and every `Material:` path in Programa to the new names. Preserve the student's manual notes
- **Missing** → run the `u-flow:init` skill flow now: the discovery already happened (cronograma, programa, unidades), so generate the CLAUDE.md directly from it
- Verify: every path referenced in CLAUDE.md must exist on disk after the move

### 6. Report

```markdown
## Organización completa: [materia]

- X archivos movidos, Y renombrados
- content/primer-parcial: N archivos | content/segundo-parcial: M
- consignas: K | entregas: J
- Sin clasificar (no tocados): [lista]
- CLAUDE.md: actualizado ✓ / creado ✓
```

## Edge Cases

- **Already organized / re-run**: detect existing `content/` structure; only new unplaced files get classified and inserted in order (renumber if needed, show the renumbering in the plan)
- **Multiple materias mixed in one folder**: stop and propose splitting first — never guess which materia a file belongs to
- **No cronograma found anywhere**: ask the user for the parcial boundaries ("¿Qué unidades entran en el primer parcial?") before planning
- **Files outside the materia folder are never touched**

## Quality Check

Before reporting done:
- Every file from the inventory is accounted for: moved, or explicitly listed as unclassified
- No file lost: count before == count after
- Names follow the convention (kebab-case, numbered, no accents/spaces)
- CLAUDE.md paths match the real disk layout
- Original folder has no leftover stragglers except unclassified ones
