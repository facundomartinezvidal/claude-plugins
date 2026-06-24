---
name: compare
description: Generate a comparison table between two or more concepts, theories, or methods from course material. Warns if content for any concept is missing.
argument-hint: "[concept A] vs [concept B] [vs concept C...]"
allowed-tools: Read, WebFetch, Bash
---

Compare two or more concepts from course material in a structured table.

## Setup

Run workflow-core steps W1, W2, W3.
W3 missing-content warnings apply per concept — flag individually if material is missing for one concept but not another.

## Step 1 — Identify comparison scope

For each concept to compare, find its definition and class/unit from the loaded context.

If a concept is not in the material or cronograma at all:
> ⚠️ [Concepto X]: no aparece en el material de la materia. ¿Querés que lo incluya igual?

## Step 2 — Identify comparison dimensions

From the course material, extract the relevant dimensions to compare across:
- Definitions as stated in the material
- Key properties or characteristics
- When/why each is used
- Advantages and limitations
- Examples from the course
- Relationships between them (is one a subset of the other? do they conflict?)

Prioritize dimensions that the professor emphasized in the material.

## Step 3 — Generate comparison table

```markdown
## Comparación: [Concept A] vs [Concept B] [vs Concept C]

| Dimensión | [Concept A] | [Concept B] | [Concept C if applicable] |
|-----------|-------------|-------------|---------------------------|
| Definición | … | … | … |
| Características | … | … | … |
| Cuándo se usa | … | … | … |
| Ventajas | … | … | … |
| Limitaciones | … | … | … |
| Ejemplo (del material) | … | … | … |
| Relación con los otros | … | … | … |

[Source: Clase NN — filename for each concept]
```

## Step 4 — Add synthesis

After the table, add a brief synthesis:

```
📌 Resumen:
[2–3 sentences explaining the key difference and when to choose each]

⚠️ Error común: [typical confusion students have between these concepts, if detectable from material]
```

Ask: "¿Querés que profundice en alguna dimensión o agregue otro concepto a la comparación?"
