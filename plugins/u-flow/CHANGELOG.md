# Changelog

All notable changes to the u-flow plugin.

Format: [Keep a Changelog](https://keepachangelog.com/). Versioning: [SemVer](https://semver.org/).

## [0.3.0] — 2026-06-05

### Added

- **`init` skill + `/u-flow:init`** — set up a CLAUDE.md per materia: cronograma (parciales, entregas, final), programa split into unidades/temas mapped to material files, bibliografía, folder layout. Update mode preserves the student's manual notes. Offers to organize a messy folder first so the CLAUDE.md is born pointing at the clean layout
- **`organize` skill + `/u-flow:organize`** — turn a messy course folder into `content/` split by parcial, files sequenced by cursada and renamed to `NN-tipo-descripcion.ext`. Mandatory plan mode (full old→new mapping approved before anything moves), no-file-lost verification, grouping heuristic (3+ files per contenido → subfolder). Creates the CLAUDE.md via init when missing
- **`presentations` skill** — slide content + speaker notes extracted from academic-writing into its own skill, with oral-defense and group-presentation adaptations and a Marp fallback
- **`reader` agent** — reads course material (PDFs, apuntes, bibliografía) in an isolated context and returns only relevant extracts with file/page citations. Used by every content skill; discovers cronograma/programa and starts from the materia's CLAUDE.md
- **`reviewer` agent** — verifies any generated artifact (informe, presentación, resumen, flashcards, diagrama) against the original sources: misattributed claims, invented citations, altered definitions, missing core concepts, consigna/lineamientos/rúbrica compliance. Findings by severity with draft-vs-source quotes; never rewrites
- **Material-first sourcing** in every content skill: pasted content → course files (via reader) → web/general knowledge only for gaps, always flagged
- **Consigna discovery** — documents and presentations find the consigna/lineamientos PDF in the workspace before asking, extract requirements (extensión, formato, carátula, rúbrica), and honor them over skill defaults
- **Plan mode** for documents and presentations: structure approved before any content is written
- **Output placement** — saved artifacts land in the materia structure: `estudio/` for study material, `entregas/` for deliverables, kebab-case names, grouping when a tema/TP accumulates 2+ artifacts
- Oral exam (coloquio) modes in summarize and study-aids; multi-source summarization; interactive quiz exit phrases

### Changed

- **`academic-writing` renamed to `documents`** — owns written work only (informes, TPs, monografías, parciales domiciliarios); slides moved to presentations
- **`study-planner` agent renamed to `planner`**, `material-scout` renamed to `reader` — one-word agent names by function
- research search strategy: SciELO/Redalyc/Dialnet + institutional repositories instead of `site:scholar.google.com`; fetch-before-citing; DOI capture; paywall guidance
- diagrams: corrected Excalidraw schema (bound text elements, reciprocal arrow bindings, full base fields — output now actually pastes into excalidraw.com); Mermaid promoted to default secondary output
- flashcards: numbered-list format (renders on mobile) instead of Markdown tables
- All skills reviewed by skill-reviewer agents; consistency fixes across calibration ranges, trigger phrases, and cross-skill handoffs

### Fixed

- Agent frontmatter: `tools` → `allowedTools`, namespaced agent names
- research: broken handoff to a nonexistent command; step numbering
- GitHub repo path in docs and manifests (`fmartinezvidal` → `facundomartinezvidal`)

## [0.1.0] — 2026-05-08

### Added

- Initial release: summarize, study-aids, diagrams, research, academic-writing skills; 6 commands; study-planner agent. Marketplace layout under `plugins/u-flow/`
