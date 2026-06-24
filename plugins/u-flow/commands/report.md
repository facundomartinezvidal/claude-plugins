---
name: u-flow:report
description: Write or scaffold an academic report in Spanish following university structure
argument-hint: "[tema del informe] — opcionalmente pegá la consigna"
allowed-tools:
  - Read
  - Glob
  - Grep
  - EnterPlanMode
  - ExitPlanMode
---

Load the u-flow:documents skill and produce an academic report.

The user has invoked `/u-flow:report` with the following arguments: 

## Instructions

1. Parse arguments for:
   - Topic or title of the report
   - Any consigna (assignment prompt) pasted by the user
2. If a consigna is present, parse it first: extract required sections, word count, citation style, and format requirements
3. If no consigna was pasted, **search the workspace before asking** (per the documents skill): Glob `**/*.pdf`, `**/*.docx`, `**/*.md`; Grep for `consigna`, `TP`, `lineamientos`, `pautas`, `rúbrica`. If found, confirm: "Encontré `[archivo]` — ¿es la consigna de este trabajo?"
4. If nothing is found, ask: "¿Tenés una consigna específica o trabajamos con la estructura estándar (Introducción, Desarrollo, Conclusión, Bibliografía)?"
5. Enter plan mode (EnterPlanMode) and build the document plan: estructura completa, qué requisito de la consigna cubre cada sección, extensión estimada, estilo de citado
6. Present the plan via ExitPlanMode and wait for approval, then write section by section
7. Apply the u-flow:documents skill: formal tone, logical connectors, citation placeholders where needed
8. At the end, ask: "¿Querés que ajuste alguna sección o que busque fuentes sobre este tema?"

## Output

Full Markdown report with headings. Always respond in Spanish. Flag citation needs with [CITA NECESARIA].
