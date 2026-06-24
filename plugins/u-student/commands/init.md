---
name: init
description: Initialize a subject workspace — collects subject metadata, reads the real cronograma and programa if available, creates the folder structure, and generates a CLAUDE.md so every future session in this folder has full subject context automatically.
argument-hint: "[subject-name]"
allowed-tools: Bash, Write, Read, WebFetch
---

Initialize a subject (materia) workspace. Run from the directory where the subject folder should be created.

## Step 1 — Check for existing workspace

Check if a folder with the subject name already exists:
```bash
ls -d */ 2>/dev/null
```

If a folder matching the name exists and has a `CLAUDE.md`, ask:
```
Ya existe una carpeta para esta materia con configuración.
¿Qué querés hacer?
1. Actualizar el cronograma o programa (agrega clases sin borrar nada)
2. Reinicializar desde cero (borra y recrea la estructura)
3. Cancelar
```

If option 1 → skip to Step 4 (cronograma) and update only.
If option 2 → proceed from Step 2.
If option 3 → stop.

## Step 2 — Collect subject metadata

Ask the following questions one by one (not all at once):

```
¿Cuál es el nombre de la materia?
```
```
¿Nombre del profesor/a?  (Enter para omitir)
```
```
¿Carrera y año de cursada? Ej: "Ingeniería en Sistemas, 2do año"  (Enter para omitir)
```
```
¿Año y cuatrimestre? Ej: "2025 — 1er cuatrimestre"  (Enter para omitir)
```
```
¿Cuántos parciales tiene la materia?
1. Dos parciales
2. Un parcial + final
3. Tres parciales
4. Solo final / coloquio
5. Otro: ___
```

Store all answers. Derive the kebab-case folder name from the subject name.
Example: "Análisis Matemático II" → `analisis-matematico-ii`

## Step 3 — Create folder structure

Create directories based on parcial count:

**2 parciales (default):**
```
{subject-name}/
├── CLAUDE.md
├── cronograma.md
├── programa.md
└── content/
    ├── primer-parcial/
    └── segundo-parcial/
```

**1 parcial + final:**
```
{subject-name}/
├── CLAUDE.md
├── cronograma.md
├── programa.md
└── content/
    ├── parcial/
    └── final/
```

**3 parciales:**
```
{subject-name}/
├── CLAUDE.md
├── cronograma.md
├── programa.md
└── content/
    ├── primer-parcial/
    ├── segundo-parcial/
    └── tercer-parcial/
```

**Solo final:**
```
{subject-name}/
├── CLAUDE.md
├── cronograma.md
├── programa.md
└── content/
```

## Step 4 — Load cronograma

Ask:
```
¿Tenés el cronograma de la materia?
1. Sí, lo pego ahora (texto, tabla, lista)
2. Sí, tengo un archivo (PDF, imagen, URL) — compartilo
3. No, lo completo después
```

If provided (options 1 or 2):
- Use content-reader skill to extract the schedule
- Parse into a table: Clase | Tema | Parcial | Archivo
- Detect class numbers, topics, and which partial each belongs to
- Populate `cronograma.md` with the real data

If not provided (option 3):
- Create `cronograma.md` with empty template based on parcial count

`cronograma.md` format:
```markdown
# Cronograma — {Subject Name}
> Año: {year} | Profesor: {professor} | {quarter}

## Primer Parcial

| Clase | Tema | Archivo |
|-------|------|---------|
| 01    | [from real cronograma or empty] | 01-[tema].ext |

## Segundo Parcial

| Clase | Tema | Archivo |
|-------|------|---------|
```

## Step 5 — Load programa

Ask:
```
¿Tenés el programa de la materia?
1. Sí, lo pego ahora
2. Sí, tengo un archivo o URL — compartilo
3. No, lo completo después
```

If provided:
- Use content-reader skill to extract units, topics, and bibliography
- Populate `programa.md` with real structure

If not:
- Create `programa.md` with empty template

`programa.md` format:
```markdown
# Programa — {Subject Name}

## Unidades

### Unidad 1 — [Nombre]
- Tema 1
- Tema 2

## Bibliografía obligatoria
## Bibliografía complementaria
```

## Step 6 — Generate CLAUDE.md

Create `CLAUDE.md` in the subject folder root. This file loads automatically in every Claude session inside this folder and gives full context to all u-student commands.

```markdown
# {Subject Name}

## Contexto de la materia

- **Materia:** {Subject Name}
- **Profesor/a:** {professor or "No especificado"}
- **Carrera:** {career or "No especificada"}
- **Cursada:** {year + quarter or "No especificado"}
- **Parciales:** {parcial structure}

## Organización del material

El contenido de la materia está organizado en la carpeta `content/` dividido por período de evaluación:

{based on parcial count, e.g.:}
- `content/primer-parcial/` — clases 01 a [N] del cronograma
- `content/segundo-parcial/` — clases [N+1] en adelante

### Convención de nombres de archivos

Todos los archivos de contenido siguen el formato: `{NN}-{nombre-del-tema}.{ext}`

Ejemplos:
- `01-introduccion.pdf`
- `05-transformada-de-laplace.md`
- `08-redes-de-petri.jpg`

El número `NN` corresponde al número de clase en el `cronograma.md`.

## Archivos de referencia

- `cronograma.md` — tabla de clases con temas y archivos correspondientes
- `programa.md` — programa oficial de la materia con unidades y bibliografía

## Estructura actual de la carpeta

```
{subject-name}/
{generate actual folder tree with: find . -not -path '*/.git/*' | sort | sed 's|[^/]*/|  |g'}
```

## Instrucciones para Claude

Cuando trabajes en esta materia:
1. **Siempre priorizá el contenido propio de la materia** sobre conocimiento general
2. Consultá `cronograma.md` para identificar a qué clase pertenece cada tema
3. Consultá `programa.md` para entender la profundidad y el enfoque esperado
4. Si un tema está en el cronograma pero no tiene archivo en `content/`, avisale al estudiante
5. Los resúmenes, presentaciones y documentos deben usar la terminología del material de la materia
6. Respondé siempre en español salvo que el estudiante escriba en otro idioma
7. Si el estudiante trabaja desde una carpeta sin `CLAUDE.md`, recomendá correr `/u-student:init` para configurar el workspace de la materia
```

## Step 7 — Git setup (automatic detection)

Check if git is installed:
```bash
which git 2>/dev/null
```

**If git NOT found:** skip entirely — do not mention git or ultraplan. The student will use standard plan mode in all commands. No action needed.

**If git found:** ask:
```
¿Querés usar control de versiones para esta materia?
Esto activa el modo ultraplan en los comandos (recomendado).
(s/n)
```

If yes:
```bash
cd {subject-name} && git init
```

Tell the student:
```
✅ Git inicializado. Para activar ultraplan completamente:
   1. Agregá al menos un archivo de contenido
   2. Corré: git add . && git commit -m "init: {subject-name}"
   A partir del primer commit, todos los comandos usarán ultraplan automáticamente.
```

If no → skip. Standard plan mode will be used.

## Step 8 — Summary

Tell the student:

```
✅ Workspace creado: {subject-name}/

📁 Estructura:
{show actual folder tree}

📋 Cronograma: {populated with X classes / plantilla vacía para completar}
📖 Programa: {populated / plantilla vacía para completar}

📌 Próximos pasos:
1. Agregá tus materiales en content/{primer-parcial|segundo-parcial}/
   Nombre: {NN}-{tema}.{ext}  →  ej: 01-introduccion.pdf

2. Completá el cronograma si quedó vacío:
   Abrí cronograma.md y completá la tabla con los temas de cada clase

3. Cuando tengas material listo, desde la carpeta de la materia usá:
   /u-student:summarize    — resumir contenido
   /u-student:study        — entender un tema
   /u-student:exam         — simular un parcial
   /u-student:write        — crear un TP o informe
```
