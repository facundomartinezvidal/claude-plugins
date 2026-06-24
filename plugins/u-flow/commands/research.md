---
name: u-flow:research
description: Research a topic prioritizing the user's course material, falling back to web search, in Spanish
argument-hint: "[tema a investigar]"
allowed-tools:
  - Read
  - Glob
  - Grep
  - WebSearch
  - WebFetch
---

Load the u-flow:research skill and research the user's topic.

The user has invoked `/u-flow:research` with the following arguments: 

## Instructions

1. Parse the topic from arguments
2. If no topic provided, ask: "¿Qué tema querés investigar? Podés indicar también si es para un informe, presentación, o estudio general."
3. Identify the research angle: definition, historical context, current state, comparison, or application
4. **Search the user's material first**: content pasted in chat, then workspace files (Glob for `**/*.pdf`, `**/*.md`, `**/*.docx`, `**/*.txt`; Grep for the topic). If course material covers the topic, build primarily from it and cite files by name
5. Use WebSearch only for what the user's material does not cover (follow research skill guidance on query strategy)
6. Triangulate at least 3 sources total (material + web) before synthesizing
7. Apply the research skill output format: Resumen General, Según tu Material de Cátedra (si hay), Contexto, Conceptos Clave, Estado Actual, Fuentes
8. Cite course material by file name; web claims with URLs and dates
9. End with a "Para tu Trabajo" section connecting findings to the user's academic need
10. Offer: "¿Querés que profundice algún aspecto o que busque fuentes en inglés también?"

## Output

Structured Markdown research summary. Course material cited by file name, web sources by URL. Always respond in Spanish.
