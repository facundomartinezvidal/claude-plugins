---
name: u-flow:research
description: This skill should be used when the user needs to research an academic topic, find sources for a report or bibliography, or gather background information before writing. Triggers on phrases like "investigá sobre", "buscame información sobre", "necesito saber más de", "explicame [tema] para un trabajo", "fuentes para la bibliografía", "tengo que hacer un marco teórico", "buscame autores sobre", "antecedentes del tema", "qué dice la literatura sobre", or when the user asks about a topic specifically in the context of university work. Searches the user's own course material first (apuntes, PDFs, bibliografía de cátedra in the workspace or pasted in chat), then falls back to web search. Responds in Spanish by default.
allowed-tools:
  - Read
  - Glob
  - Grep
  - Write
  - WebSearch
  - WebFetch
---

# Research — Academic Topic Investigation

## Purpose

To research academic topics and deliver structured, sourced summaries suitable for university work. **Prioritize the user's own course material** (apuntes, PDFs, bibliografía de cátedra) over the web — professors evaluate against what they taught, not against generic internet sources. Apply when the user needs to understand a topic, gather sources, or get background information before writing or studying.

## Core Principles

- Respond in **Spanish** unless the user explicitly requests another language
- **Course material first, web second** — what the cátedra uploaded outranks any web source
- Always cite sources — academic research requires attribution
- Triangulate: use at least 2-3 sources before presenting a conclusion
- Flag uncertainty: distinguish confirmed facts from contested claims
- Prefer academic and reputable sources over general websites

## Research Process

1. **Clarify the query** — if the topic is vague, identify the specific angle the user needs:
   - Historical background?
   - Current state of research?
   - Key figures and their positions?
   - Practical applications?

2. **Search the user's material FIRST** — before touching the web, check what the user already has:
   - Content pasted in the conversation (apuntes, transcripciones, consignas)
   - Files in the workspace: use Glob to find candidates (`**/*.pdf`, `**/*.md`, `**/*.docx`, `**/*.txt`) and Grep to locate the topic inside them
   - Folders that look like course material: `apuntes/`, `materias/`, `bibliografia/`, subject names
   - Read the relevant files and extract definitions, authors, and frameworks **as the cátedra presents them**
   - **For large files or multiple PDFs, delegate to the `u-flow:reader` agent** — it reads in an isolated context and returns only the relevant extracts with file/page citations
   - If material covers the topic, build the answer primarily from it and cite each file by name

3. **Fall back to web search** — only for what the user's material does not cover (gaps, context, contrasting positions). Use targeted queries:
   - `"[topic]" site:*.scielo.org` for Ibero-American peer-reviewed papers (content lives on country subdomains: scielo.org.ar, scielo.br, etc.)
   - `"[topic]" site:redalyc.org` for Latin American academic journals
   - `"[topic]" site:dialnet.unirioja.es` for Spanish-language indexing (strong for humanities)
   - `"[topic]" repositorio universidad` for institutional open-access repositories
   - `"[topic]" definición OR concepto` for definitions
   - `"[topic]" historia OR origen` for historical context

4. **Fetch before citing** — use WebFetch on the most promising results to read the actual page. Search snippets alone are not enough to extract accurate claims, dates, or a DOI.

5. **Synthesize results** — do not paste search results raw; extract key information and structure it. When course material and web sources disagree, present the cátedra's version first and note the difference.

6. **Cite every claim** — course material by file name ("según el apunte `unidad-3.pdf`"); web sources by URL with publication date and DOI when available

7. **Identify gaps** — note what is disputed, uncertain, or requires deeper investigation

## Output Format

```markdown
# Investigación: [Tema]

## Resumen General
[3-5 oraciones que describan el tema con precisión]

## Según tu Material de Cátedra   ← incluir solo si el usuario tiene material
[Qué dicen los apuntes/PDFs del usuario sobre el tema, con los marcos y autores que usa la cátedra]
Fuente: [nombre del archivo]

## Contexto y Origen
[Cuándo surgió, quién lo desarrolló, por qué importa]
Fuente: [URL / Autor, Año]

## Conceptos Clave
- **[Término 1]**: [Definición]
- **[Término 2]**: [Definición]
Fuente: [URL]

## Estado Actual / Debate Académico
[Lo que la literatura reciente dice; posiciones enfrentadas si las hay]
Fuente: [URL]

## Aplicaciones o Ejemplos
[Cómo se aplica en la práctica o casos concretos]
Fuente: [URL]

## Fuentes Recomendadas para Profundizar
1. [Título] — [URL] — [Por qué es útil]
2. [Título] — [URL] — [Por qué es útil]

## Para tu Trabajo
[Sugerencia de cómo usar esta información en el informe o presentación]
```

## Source Evaluation

### Prefer (in order)
0. **The user's own course material** (apuntes, PDFs de la cátedra, bibliografía obligatoria) — always first
1. University and academic institution websites (.edu, .ac)
2. Peer-reviewed journals (Google Scholar, JSTOR, SciELO, Redalyc)
3. Government and official organizations
4. Established encyclopedias (Britannica, RAE, Stanford Encyclopedia of Philosophy)
5. Quality journalism (for current events and applied topics)

### Flag or Avoid
- Wikipedia as a primary source (use as entry point, then follow citations)
- Sites without clear authorship or date
- Content older than 5 years for social sciences, economics, and technology (flag if used); older than 10 years for humanities and philosophy
- Blogs and opinion pieces (usable for perspective, not as factual authority)

## Handling Different Research Needs

### Conceptual Understanding ("¿qué es X?")
- Lead with a clear, precise definition
- Explain why the concept matters in its field
- Provide 2-3 concrete examples

### Historical Research ("origen de X", "historia de X")
- Organize chronologically
- Focus on turning points and key figures
- Connect past to present implications

### Comparative Research ("diferencia entre X e Y")
- Use a comparison table as the lead
- Then expand with nuanced discussion per item

### For a Specific Work / Report
- Tailor research directly to the consigna
- Identify which aspects of the topic are most relevant to the assignment
- Suggest which sources to cite and how

## When WebSearch Returns Insufficient Results

If search results are thin or off-topic:
1. Try alternative search terms (synonyms, Spanish + English variations)
2. Narrow the scope: "Parece que hay poca información sobre este tema en español — ¿querés que busque en inglés también?"
3. Offer what is known from training data, clearly labeled: "Basado en mi conocimiento previo (sin fuentes consultadas en esta búsqueda):"

## Paywall Handling

If the full text of an important source is behind a paywall:
- Note it explicitly: "Artículo disponible con acceso institucional"
- Suggest alternatives: biblioteca digital de la universidad, SciELO, Redalyc, or ResearchGate for author-posted preprints
- Use the abstract and any available preview to extract what's citable

## Handoff to Writing

When the user has enough sources and signals readiness to write ("listo", "empecemos con el informe", "ya tengo lo que necesito"), offer: "¿Empezamos a redactar el trabajo? Usá `/u-flow:report` para un informe o `/u-flow:presentation` para diapositivas, y armamos el documento con estas fuentes."

## Saving Output

Deliver in chat by default. When the user wants the result saved (or accepts the offer to save it):
- Location: `[materia]/estudio/` — follow the Estructura in the materia's CLAUDE.md if it defines a different layout
- Name: `investigacion-[tema].md`, kebab-case, no accents — e.g. `estudio/investigacion-keynesianismo.md`
- If the same tema accumulates 2+ artifacts (resumen + flashcards + diagrama), group them: `estudio/[tema]/resumen.md`, `estudio/[tema]/flashcards.md` — move the existing loose artifact in when creating the second
- Create the folder if it doesn't exist; never overwrite an existing file without confirming

## Quality Check

Before finalizing:
- The user's material was checked before searching the web (or none exists)
- Every factual claim has a source: file name for course material, URL for web
- Sources are evaluated for credibility
- Contested claims are flagged as such
- The output is useful for the specific academic task the user described
- "Para tu Trabajo" section connects research to the user's actual need
