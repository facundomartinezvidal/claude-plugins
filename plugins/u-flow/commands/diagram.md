---
name: u-flow:diagram
description: Create an Excalidraw mind map or flowchart from academic content
argument-hint: "[tipo: mapa mental | flowchart | mapa conceptual] [tema]"
allowed-tools:
  - Read
  - Glob
  - Grep
---

Load the u-flow:diagrams skill and generate an Excalidraw diagram from the user's content.

The user has invoked `/u-flow:diagram` with the following arguments: 

## Instructions

1. Parse arguments to determine:
   - Diagram type (if specified): "mapa mental", "flowchart", "mapa conceptual", "timeline", "árbol"
   - Topic or content to diagram
2. If only a topic is named (no content pasted), **search the workspace first** via the u-flow:reader agent — it reads course PDFs/apuntes in isolation and returns the relevant concepts and relationships. Build the diagram from the cátedra's material
3. If no content exists anywhere, ask: "¿Qué querés diagramar? Pegá el texto o describí el tema y el tipo de diagrama."
4. If no type is specified, choose the best type based on content structure (see diagrams skill guidance)
5. Apply the diagrams skill: generate valid Excalidraw JSON with proper coordinates and connections
6. After the JSON block, provide:
   - Instructions on how to import into Excalidraw
   - Brief text description of the diagram structure
7. Offer: "¿Querés que expanda alguna rama o cambie el tipo de diagrama?"

## Output

Valid Excalidraw JSON in a fenced code block, followed by import instructions. Always respond in Spanish.
