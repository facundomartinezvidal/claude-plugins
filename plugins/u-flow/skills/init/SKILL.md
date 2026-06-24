---
name: u-flow:init
description: This skill should be used when the user wants to set up a materia (course) so the plugin understands it: initialize a CLAUDE.md with the cronograma, the program divided into unidades/temas, the bibliografía, and the file layout. Triggers on phrases like "inicializá la materia", "configurá la materia", "armá el claude.md de la materia", "setup de la materia", "empiezo a cursar", "nueva materia", "cargá el programa de la materia", or when the user starts working in a course folder that has no CLAUDE.md. Responds in Spanish by default.
allowed-tools:
  - Read
  - Glob
  - Grep
  - Write
  - Edit
---

# Init — Course Setup (CLAUDE.md per Materia)

## Purpose

To initialize a `CLAUDE.md` for a materia so every u-flow skill understands it automatically: cronograma (parciales, entregas, final), programa dividido en unidades/temas, bibliografía, and where the material lives. `CLAUDE.md` loads into context on every session — once initialized, research, documents, presentations, summarize, study-aids, and diagrams know the materia without re-explaining.

## Core Principles

- Respond in **Spanish** unless the user explicitly requests another language
- **Discover before asking** — the programa, cronograma, and bibliografía usually exist as PDFs in the folder
- One materia → one `CLAUDE.md` at the materia's folder root. Multiple materias in one workspace → one `CLAUDE.md` inside each materia folder
- The file is the student's — keep it editable, plain, and short; no generated noise
- Never overwrite an existing `CLAUDE.md` without showing the diff and confirming

## Process

### 1. Locate the materia

- If the user names the materia and folder, use that
- Otherwise infer from the working directory or ask: "¿Qué materia configuramos y en qué carpeta está su material?"
- Check for an existing `CLAUDE.md` → if present, switch to **Update Mode**

### 2. Discover the course documents

Delegate bulk reading to the `u-flow:reader` agent. Targets:
- **Programa / syllabus**: Grep for `programa`, `syllabus`, `contenidos`, `unidades` — defines the official tema/unidad breakdown
- **Cronograma**: Grep for `cronograma`, `calendario`, `fechas`, `parcial`, `entrega`, `final` — exam and submission dates
- **Bibliografía**: Grep for `bibliografía`, `lecturas`, `obligatoria` — required and complementary readings
- **Material files**: Glob the folder and map files to unidades by name (`unidad-3.pdf`, `clase-05.pdf`, `tp1.pdf`)

### 3. Organize first if the folder is messy

If the folder lacks the `content/` structure (loose, inconsistently named files), offer to organize before generating: "La carpeta está desordenada — ¿la organizo primero (content/ por parcial, archivos renombrados) así el CLAUDE.md ya nace apuntando a la estructura limpia?"

- **Accepted** → run the `u-flow:organize` skill flow (reuse the discovery already done — inventory and cronograma are in hand), then generate the CLAUDE.md against the organized layout
- **Declined** → generate against the current layout; note in Estructura that `/u-flow:organize` is available

### 4. Interview for the gaps

Ask only what discovery did not answer, in ONE compact question block:
- Carrera / universidad / cuatrimestre
- Modalidad de evaluación (parciales, final, TPs, promoción)
- Fechas que no estén en ningún PDF
- Hasta qué tema llegó la cursada (estado actual)

### 5. Generate CLAUDE.md

Use this template, filling only what is known — omit empty sections:

```markdown
# [Materia]

Materia de [carrera], [universidad]. [Cuatrimestre/año]. Cátedra [nombre].
Evaluación: [parciales / final / TPs / promoción].

## Cronograma

| Fecha | Evento |
|---|---|
| [fecha] | Parcial 1 — unidades 1-3 |
| [fecha] | Entrega TP2 |
| [fecha] | Final |

## Programa

### Unidad 1 — [Nombre]
- Temas: [tema, tema, tema]
- Material: `[archivo.pdf]`, `[archivo2.pdf]`
- Estado: vista

### Unidad 2 — [Nombre]
- Temas: [...]
- Material: `[...]`
- Estado: en curso

## Bibliografía

- **Obligatoria:** [Autor, obra] — `[archivo.pdf si existe]`
- **Complementaria:** [...]

## Estructura

- `content/` — material de la cátedra por parcial
- `consignas/` — consignas y lineamientos de TPs
- `estudio/` — material generado (resúmenes, flashcards, diagramas)
- `entregas/` — trabajos propios y entregables

## Notas para u-flow

- Citar siempre con los marcos y autores de esta cátedra
- [preferencias del estudiante: estilo de resumen, formato de citado pedido por la cátedra, etc.]
```

### 6. Confirm and write

- Show the generated content before writing
- Write to `[carpeta-materia]/CLAUDE.md`
- Close with: "Listo — ahora todas las skills de u-flow van a entender [materia]. Actualizalo cuando avance la cursada o pedime `/u-flow:init` de nuevo."

## Update Mode

If `CLAUDE.md` already exists:
1. Read it and detect what changed: new files in the folder, dates passed, new unidades covered
2. Propose specific updates ("La unidad 3 tiene material nuevo: `clase-08.pdf` — ¿la agrego? ¿Marcamos la unidad 2 como vista?")
3. Apply with Edit — never regenerate the whole file (the student's manual notes must survive)

## How the Rest of u-flow Uses This

`CLAUDE.md` loads automatically — no skill needs to read it explicitly. Once it exists:
- **research / summarize / study-aids / diagrams** know which unidad a topic belongs to and which files cover it
- **documents / presentations** know the cátedra, the citation style, and where consignas live
- **planner** builds study plans against the real cronograma (días hasta el parcial) and the unidades pending
- **reader** gets better search targets from the Estructura section

## Quality Check

Before finalizing:
- Every unidad in the programa has its temas listed
- Material files are mapped to unidades where the mapping is clear
- Dates are absolute (with year), not relative
- The file stays under ~80 lines — it loads on every session, brevity is a feature
- Existing user content was preserved (Update Mode)
