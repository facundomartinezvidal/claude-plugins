---
name: u-flow:organize
description: Organize a materia's files into content/ split by parcial, ordered and renamed consistently
argument-hint: "[materia o carpeta]"
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

Load the u-flow:organize skill and organize the materia's folder.

The user has invoked `/u-flow:organize` with the following arguments: 

## Instructions

1. Parse arguments for the materia/folder; infer from the working directory if absent
2. **Discover** via the u-flow:reader agent: full file inventory, cronograma/programa (they define parcial boundaries and tema order), existing CLAUDE.md. Have the reader open ambiguous files to identify them
3. **Classify** every file: parcial, order in the cursada, tipo (material/práctica/consigna/entrega). Ask about unclassifiable files in ONE batch
4. **Plan in plan mode** (EnterPlanMode): full old→new mapping per the skill's target structure and naming convention (`NN-tipo-descripcion.ext`). Every file appears — destination or "no se mueve". Present via ExitPlanMode; move NOTHING until approved
5. **Execute**: mkdir + mv per the approved mapping; flag name collisions as probable duplicates
6. **Update CLAUDE.md**: fix Estructura and Material paths to the new names; if no CLAUDE.md exists, create it now via the u-flow:init skill (discovery is already done)
7. **Report**: files moved/renamed, per-folder counts, unclassified files untouched, CLAUDE.md status

## Output

Organized folder + verification that no file was lost (count before == after). Always respond in Spanish.
