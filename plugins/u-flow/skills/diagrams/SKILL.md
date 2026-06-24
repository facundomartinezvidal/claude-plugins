---
name: u-flow:diagrams
description: This skill should be used when the user wants to create visual diagrams from academic content for study or presentation purposes. Triggers on phrases like "haceme un mapa mental", "diagrama de flujo", "mapa conceptual", "esquema para estudiar", "graficá esto", "representá en un diagrama", "organizá visualmente", "mapa para el parcial", "esquema visual", "diagrama de causa y efecto", or when the user provides content and asks for a visual or graphic representation. If no content is pasted, looks for course material in the workspace via the reader agent. Responds in Spanish by default.
allowed-tools:
  - Read
  - Glob
  - Grep
  - Write
---

# Diagrams — Academic Visual Diagrams (Excalidraw)

## Purpose

To generate visual diagrams from academic content as Excalidraw-compatible JSON. Apply when the user wants to visualize concept relationships, processes, hierarchies, or timelines for study or presentation purposes.

## Core Principles

- Respond in **Spanish** unless the user explicitly requests another language
- Every diagram should be pasteable directly into Excalidraw
- Clarity over complexity — a diagram with 10 clear nodes beats 30 cluttered ones
- Choose diagram type based on content structure
- If the user specifies a type, use it regardless of what the content suggests
- Color palette is Catppuccin-inspired by default — if the user wants neutral academic colors, use white/gray backgrounds with dark stroke

## Choosing the Right Diagram Type

| Content Type | Recommended Diagram |
|---|---|
| Topic with subtopics, ideas branching out | Mind map |
| Relationships between defined concepts | Concept map |
| Step-by-step process or algorithm | Flowchart |
| Historical events in order | Timeline |
| Categories with members | Hierarchy tree |
| Cause → Effect chains | Causal diagram |

## Excalidraw Output Format

Always output a valid Excalidraw JSON block. Wrap it in a fenced code block labeled `json`:

**Critical rules for valid output:**

1. Text inside a shape is a **separate `text` element** with `containerId` pointing at the shape. Never put `text`/`fontSize`/`textAlign` on the shape itself — they are ignored.
2. The shape must list its text in `boundElements`. Excalidraw recomputes the text's `x`/`y` to center it, so approximate coordinates are fine — do not hand-calculate exact centering.
3. **Bindings are reciprocal.** When an arrow binds to two shapes, each shape must also list that arrow in its own `boundElements`. Without the back-reference the binding breaks when the node moves.
4. Every element needs the base fields shown below (`angle`, `groupIds`, `roundness`, `seed`, `versionNonce`, `isDeleted`, etc.). Vary `seed` and `versionNonce` per element (use any distinct integers).

This example has two nodes, bound labels, and a connecting arrow — a complete, pasteable template to imitate:

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [
    {
      "id": "node-1", "type": "ellipse",
      "x": 400, "y": 300, "width": 200, "height": 80,
      "angle": 0, "strokeColor": "#1e1e2e", "backgroundColor": "#cba6f7",
      "fillStyle": "solid", "strokeWidth": 2, "strokeStyle": "solid",
      "roughness": 1, "opacity": 100, "groupIds": [], "frameId": null,
      "roundness": null, "seed": 1001, "version": 1, "versionNonce": 11,
      "isDeleted": false, "link": null, "locked": false,
      "boundElements": [{"type": "text", "id": "text-1"}, {"type": "arrow", "id": "arrow-1-2"}]
    },
    {
      "id": "text-1", "type": "text",
      "x": 430, "y": 330, "width": 140, "height": 25,
      "angle": 0, "strokeColor": "#1e1e2e", "backgroundColor": "transparent",
      "fillStyle": "solid", "strokeWidth": 2, "strokeStyle": "solid",
      "roughness": 1, "opacity": 100, "groupIds": [], "frameId": null,
      "roundness": null, "seed": 1002, "version": 1, "versionNonce": 12,
      "isDeleted": false, "link": null, "locked": false,
      "text": "Concepto Central", "originalText": "Concepto Central",
      "fontSize": 18, "fontFamily": 1, "textAlign": "center",
      "verticalAlign": "middle", "lineHeight": 1.25, "baseline": 18,
      "containerId": "node-1", "boundElements": []
    },
    {
      "id": "node-2", "type": "rectangle",
      "x": 400, "y": 480, "width": 200, "height": 70,
      "angle": 0, "strokeColor": "#1e1e2e", "backgroundColor": "#89b4fa",
      "fillStyle": "solid", "strokeWidth": 2, "strokeStyle": "solid",
      "roughness": 1, "opacity": 100, "groupIds": [], "frameId": null,
      "roundness": null, "seed": 1003, "version": 1, "versionNonce": 13,
      "isDeleted": false, "link": null, "locked": false,
      "boundElements": [{"type": "text", "id": "text-2"}, {"type": "arrow", "id": "arrow-1-2"}]
    },
    {
      "id": "text-2", "type": "text",
      "x": 430, "y": 505, "width": 140, "height": 25,
      "angle": 0, "strokeColor": "#1e1e2e", "backgroundColor": "transparent",
      "fillStyle": "solid", "strokeWidth": 2, "strokeStyle": "solid",
      "roughness": 1, "opacity": 100, "groupIds": [], "frameId": null,
      "roundness": null, "seed": 1004, "version": 1, "versionNonce": 14,
      "isDeleted": false, "link": null, "locked": false,
      "text": "Subtema", "originalText": "Subtema",
      "fontSize": 18, "fontFamily": 1, "textAlign": "center",
      "verticalAlign": "middle", "lineHeight": 1.25, "baseline": 18,
      "containerId": "node-2", "boundElements": []
    },
    {
      "id": "arrow-1-2", "type": "arrow",
      "x": 500, "y": 380, "width": 0, "height": 100,
      "angle": 0, "strokeColor": "#6c7086", "backgroundColor": "transparent",
      "fillStyle": "solid", "strokeWidth": 2, "strokeStyle": "solid",
      "roughness": 1, "opacity": 100, "groupIds": [], "frameId": null,
      "roundness": null, "seed": 1005, "version": 1, "versionNonce": 15,
      "isDeleted": false, "link": null, "locked": false,
      "points": [[0, 0], [0, 100]],
      "startBinding": {"elementId": "node-1", "focus": 0, "gap": 8},
      "endBinding": {"elementId": "node-2", "focus": 0, "gap": 8},
      "startArrowhead": null, "endArrowhead": "arrow", "boundElements": []
    }
  ],
  "appState": {
    "viewBackgroundColor": "#ffffff"
  },
  "files": {}
}
```

Every shape needs a corresponding bound text element. Add each text element immediately after its parent shape, and add every connecting arrow to the `boundElements` of both shapes it links.

## Diagram Construction Rules

### Node Types
- **Ellipse / Circle** — central topics, main concepts
- **Rectangle** — subtopics, supporting concepts
- **Diamond** — decision points (flowcharts only)
- **Arrow** — relationships and connections (type: `arrow`)

### Layout Guidelines

**Mind Map:**
- Central concept at x=400, y=300 (screen center)
- First-level branches radiate outward: top, bottom, left, right, diagonals
- Spacing: 200px between levels, 150px between sibling nodes
- Color: central node in accent color, branches in lighter shades

**Flowchart:**
- Start at top (y=50), flow downward
- Each step: 200px below the previous
- Decision diamonds with Yes/No labels on outgoing arrows
- End node at bottom

**Hierarchy Tree:**
- Root at top center
- Each level 150px below parent
- Siblings spread horizontally with 50px gap

### Color Palette (Catppuccin-inspired, readable)
- Central node: `#cba6f7` (purple) background
- Level 1: `#89b4fa` (blue) background  
- Level 2: `#a6e3a1` (green) background
- Level 3: `#f9e2af` (yellow) background
- Text: `#1e1e2e` (dark)
- Arrows: `#6c7086` (gray)

## Sourcing the Content

If the user names a topic, materia, or unidad without pasting content ("mapa mental de la unidad 2"):
1. **Check the conversation** — was content pasted earlier?
2. **Search the workspace** — delegate to the `u-flow:reader` agent: it reads the course material (PDFs, apuntes) in an isolated context and returns the relevant concepts and relationships
3. **Build the diagram from the cátedra's material** — its structure and terminology
4. **Only if nothing exists**, ask for the material or build from general knowledge, flagging it: "Armé el diagrama con conocimiento general — si tenés apuntes de la cátedra, puedo ajustarlo"

## Diagram Generation Process

1. **Parse content** — identify main topic, subtopics, and relationships
2. **Select type** — match content structure to diagram type
3. **Plan layout** — sketch node positions before writing JSON (use coordinates)
4. **Generate elements array** — nodes first, then arrows
5. **Assign IDs** — use descriptive IDs: `node-central`, `node-subtopic-1`, `arrow-1-2`
6. **Validate connections** — every arrow's `startBinding` and `endBinding` must reference real node IDs

## Arrow Coordinate Rules

(See the full arrow element in the example above for required fields.)

- `x` and `y` are the arrow's absolute start position
- `points` are **relative offsets** from that position — `[0,0]` is the start, the last point is the end
- A diagonal arrow going 150px right and 100px down: `"x": 500, "y": 340, "points": [[0,0],[150,100]]`
- `width` and `height` must match the bounding box of the points array (absolute values)
- Both shapes the arrow connects must list it in their `boundElements`

## After Generating the JSON

After the JSON block, always provide:

1. **Instructions to import:**
   ```
   Para usar en Excalidraw:
   1. Copiá el bloque JSON completo
   2. Abrí excalidraw.com (o la app de escritorio)
   3. Hacé clic en cualquier parte del lienzo y pegá con Ctrl+V (Cmd+V en Mac)
      — Excalidraw detecta el JSON del portapapeles y crea el diagrama
   ```

2. **Text description** of what the diagram shows (for users who can't load it immediately)

3. **Mermaid fallback** — always include a Mermaid version after the Excalidraw JSON. Many students use Notion, Obsidian, or VS Code which render Mermaid natively without needing Excalidraw:
   ````
   También en formato Mermaid (para Notion, Obsidian, VS Code):
   ```mermaid
   [mermaid code here]
   ```
   ````

## Handling Large Topics

If the topic has more than 15 subtopics:
1. Generate the mind map for the top 2 levels only
2. Offer to generate separate detailed diagrams for each branch
3. Note: "Este tema es extenso — generé el nivel superior. ¿Querés que profundice alguna rama?"

## Verification Pass

When the diagram was built from course material, offer a review via the `u-flow:reviewer` agent — it re-reads the sources and verifies that the relationships drawn (arrows, hierarchies) reflect what the material says, with no invented connections. Offer: "¿Verifico el diagrama contra el material de la cátedra?"

## Saving Output

Deliver in chat by default. When the user wants the result saved (or accepts the offer to save it):
- Location: `[materia]/estudio/` — follow the Estructura in the materia's CLAUDE.md if it defines a different layout
- Name: `diagrama-[tema].excalidraw`, kebab-case, no accents — e.g. `estudio/diagrama-ciclo-economico.excalidraw` (Mermaid version as `.md` alongside if asked)
- If the same tema accumulates 2+ artifacts (resumen + flashcards + diagrama), group them: `estudio/[tema]/resumen.md`, `estudio/[tema]/flashcards.md` — move the existing loose artifact in when creating the second
- Create the folder if it doesn't exist; never overwrite an existing file without confirming

## Quality Check

Before finalizing:
- All node IDs are unique
- All arrows reference valid node IDs
- No nodes overlap (check coordinate spacing)
- Central concept is clearly identifiable
- Label text is concise (max 5 words per node)
