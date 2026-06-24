---
name: mindmap
description: Generate a visual mind map of course concepts using Mermaid — organized by unit or partial, based on course material. Warns if content is missing from the cronograma.
argument-hint: "[topic, files, or --parcial 1|2]"
allowed-tools: Read, WebFetch, Write, Bash
---

Generate a mind map from course material using Mermaid diagrams.

## Setup

Run workflow-core steps W1, W2, W3.
If `--parcial 1` or `--parcial 2` provided, scope W2 to that partial only.
W3 missing-content warnings appear as empty/flagged nodes in the map.

## Step 1 — Build mind map

Organize the Mermaid mindmap with this hierarchy:
- Root: subject name (or topic if specific)
- Level 1: units from `programa.md` or parcial periods
- Level 2: classes / main topics from `cronograma.md`
- Level 3: key concepts from the material files
- Level 4: sub-concepts, definitions, examples (if content is rich enough)

Keep labels short — 3–5 words max per node.

Only include concepts found in the course material. Do not add generic nodes not present in the material.

## Step 3 — Output

Generate a Mermaid mindmap:

````markdown
```mermaid
mindmap
  root((**[Subject/Topic]**))
    [Unit 1]
      [Clase 01 — Topic]
        Concept A
        Concept B
      [Clase 02 — Topic]
        Concept C
    [Unit 2]
      [Clase 03 — Topic]
        Concept D
          Sub-concept
```
````

If content is extensive, split into multiple diagrams by unit — one per unit — to avoid overwhelming the diagram.

After the diagram(s), add a brief legend:
```
📍 Nodos en negrita: conceptos centrales del parcial
⬜ Nodos simples: conceptos derivados
⚠️ Nodos marcados: sin material disponible
```

## Step 4 — Save option

Ask: "¿Querés que guarde el mapa como archivo? (mapa-[topic].md)"
If yes, save to current directory.

Suggest: "Para visualizarlo, pegá el código en https://mermaid.live o usá la extensión Mermaid en VSCode."
