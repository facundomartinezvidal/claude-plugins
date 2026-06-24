---
name: organize
description: Organize subject material files into the correct folder structure — detects files in the current directory, maps them to classes using the cronograma, renames them to the standard convention, and moves them to primer-parcial or segundo-parcial.
argument-hint: "[optional: path to folder with files]"
allowed-tools: Read, Bash, Write
---

Organize loose subject material files into the standard u-student folder structure.

## Setup

Run workflow-core step W1 only (load CLAUDE.md and cronograma.md for context and class mapping).

If W1 finds no context files:
```
⚠️ No encontré la configuración de la materia en esta carpeta.
Se recomienda correr primero: /u-student:init

¿Querés continuar de todas formas organizando los archivos sin cronograma? (s/n)
```
If no → stop. If yes → proceed without class mapping.

## Step 1 — Scan for files

Find all non-organized files. Check:
1. Files in current directory root (excluding .md config files)
2. Files in provided path argument if given
3. Any subfolder that is NOT already `primer-parcial/` or `segundo-parcial/`

```bash
find . -maxdepth 2 -type f \
  ! -name "CLAUDE.md" ! -name "cronograma.md" ! -name "programa.md" \
  ! -path "./content/*" ! -path "./.git/*" \
  | sort
```

If no files found:
```
No encontré archivos para organizar. 
Agregá tus materiales en la carpeta de la materia y volvé a correr /u-student:organize.
```

## Step 3 — Identify and map each file

For each file found, determine:

**From filename** (if already has class number pattern `NN-*`):
- Extract class number → look up in cronograma → assign to correct parcial

**From filename without number:**
- Try to match filename keywords against cronograma topics
- If match found → propose mapping
- If no match → ask student

**Unknown files:** present a list and ask:
```
Encontré estos archivos que no pude mapear automáticamente:
  - foto-clase.jpg
  - apunte-final.pdf
  - ejercicios.docx

Para cada uno decime:
  a. Número de clase (ej: "3") o tema (ej: "transformada de laplace")
  b. O "ignorar" para dejarlo sin mover
```

## Step 4 — Propose rename and move plan

Before moving anything, show the full plan:

```
📋 Plan de organización

Archivos a mover y renombrar:

  apunte-final.pdf
    → content/primer-parcial/03-transformada-de-laplace.pdf
    (Clase 03 — Transformada de Laplace, detectado por nombre)

  foto-clase.jpg
    → content/segundo-parcial/07-redes-de-petri.jpg
    (Clase 07 — Redes de Petri, asignado manualmente)

  ejercicios.docx
    → content/primer-parcial/02-algebra-lineal.docx
    (Clase 02 — Álgebra Lineal, detectado por nombre)

Archivos a ignorar:
  - notas-personales.txt

¿Confirmás? (s / n / modificar)
```

Wait for confirmation before any file operation.

## Step 5 — Execute

On confirmation:
```bash
# Create destination folders if needed
mkdir -p content/primer-parcial content/segundo-parcial

# Move and rename each file
mv "{source}" "content/{parcial}/{NN}-{topic}.{ext}"
```

If a file with the same destination name already exists, ask:
```
Ya existe content/primer-parcial/03-transformada-de-laplace.pdf
¿Reemplazarlo, renombrarlo como -v2, o saltear?
```

## Step 6 — Update cronograma

After moving files, update the `Archivo` column in `cronograma.md` for each mapped class:

```bash
# Read current cronograma and update file references
```

## Step 7 — Update CLAUDE.md folder tree

Regenerate the folder tree section in `CLAUDE.md` to reflect the new structure:

```bash
find . -not -path '*/.git/*' | sort
```

Update the `## Estructura actual de la carpeta` section in `CLAUDE.md` with the fresh tree.

## Step 8 — Summary

```
✅ Organización completa

Archivos movidos: [N]
Archivos ignorados: [N]
Cronograma actualizado: [yes/no]

Estructura actualizada:
{show new tree}

💡 Próximos pasos:
  /u-student:summarize   — resumir el contenido organizado
  /u-student:exam        — simulacro de parcial
```
