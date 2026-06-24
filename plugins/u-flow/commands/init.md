---
name: u-flow:init
description: Initialize a CLAUDE.md for a materia so u-flow understands its cronograma, programa, and material
argument-hint: "[materia] [carpeta opcional]"
allowed-tools:
  - Read
  - Glob
  - Grep
  - Write
  - Edit
---

Load the u-flow:init skill and set up the materia's CLAUDE.md.

The user has invoked `/u-flow:init` with the following arguments: 

## Instructions

1. Parse arguments for materia name and folder; infer from the working directory if absent
2. If a `CLAUDE.md` already exists in the target folder, switch to Update Mode (per the init skill) — never regenerate over the student's notes
3. **Discover before asking**: delegate to the u-flow:reader agent to find and read the programa, cronograma, and bibliografía (Grep for `programa`, `cronograma`, `parcial`, `bibliografía`); Glob the folder to map material files to unidades
4. **If the folder is messy** (no `content/` structure), offer to organize first via the u-flow:organize skill so the CLAUDE.md is born pointing at the clean layout. Reuse the discovery already done. If declined, generate against the current layout
5. Ask ONE compact question block for whatever discovery did not answer (carrera, cuatrimestre, modalidad de evaluación, fechas, estado de la cursada)
6. Generate the CLAUDE.md from the init skill template: cronograma table, programa por unidades with temas + material + estado, bibliografía, estructura de carpetas
7. Show the content, confirm, then Write to `[carpeta]/CLAUDE.md`
8. Close: "Listo — ahora todas las skills de u-flow van a entender [materia]."

## Output

A concise `CLAUDE.md` (under ~80 lines) at the materia's folder root. Always respond in Spanish.
