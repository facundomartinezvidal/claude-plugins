---
name: content-reader
description: Internal skill for extracting and normalizing content from any academic source type — plain text, markdown files, PDFs, images of slides or notes, URLs, and audio/video transcriptions. Also loads subject context files (CLAUDE.md, cronograma.md, programa.md, progreso.md) when available. Used internally by all u-student commands and agents.
allowed-tools: Read, WebFetch, Bash
---

Extract and normalize content from academic sources. This skill is internal — do not narrate the process to the student. Just read and return normalized content.

## Subject context files (load first, always)

Before processing input content, check for and read these files if they exist:

```bash
ls CLAUDE.md cronograma.md programa.md progreso.md 2>/dev/null
```

- `CLAUDE.md` — subject metadata, folder structure, instructions
- `cronograma.md` — class schedule, topic → class number mapping
- `programa.md` — official syllabus, units, bibliography
- `progreso.md` — student's tracked progress if previously saved

Load all found context files silently. They inform every subsequent operation.

## Input types

**Text and markdown files**
- Read with Read tool
- Preserve section structure and headings

**PDFs and presentation files**
- Read with Read tool — extract all readable text
- Note slide/page numbers as context markers
- Flag figures or diagrams: `[FIGURA: descripción]`

**Images (slides, handwritten notes, whiteboards)**
- Analyze visually
- Transcribe all readable text preserving structure
- Describe diagrams and charts that carry key information
- Flag illegible sections: `[ILEGIBLE]`

**URLs**
- Fetch with WebFetch
- Extract main content, strip navigation/ads/footers
- Note source URL and retrieval date

**Audio/video transcriptions**
- Accept raw transcription text
- Clean filler words and formatting artifacts
- Reconstruct logical paragraph breaks

## Output format

For each source, produce a normalized content block:

```
[SOURCE: {filename | url | "pasted text"}]
[TYPE: {file|image|url|transcription}]
[CLASS: {NN from filename pattern, or "unknown"}]
[PARTIAL: {primer-parcial|segundo-parcial|unknown} — from directory location]
[CONTEXT: {subject name from CLAUDE.md if available}]

{extracted content}
```

## File naming detection

If filename matches `{NN}-{name}.{ext}`:
- Extract class number `NN` automatically
- Look up topic in `cronograma.md` using class number
- Map to parcial based on directory location (`primer-parcial/` or `segundo-parcial/`)

## Missing content detection

After reading all provided sources, cross-reference with `cronograma.md`:
- Identify which classes from the cronograma are covered by the provided content
- Identify which classes are NOT covered

Return a coverage summary alongside the content:
```
[COVERAGE]
Clases cubiertas: [NN, NN, NN]
Clases sin material: [NN — Tema, NN — Tema]
```

This lets the calling command decide whether to warn the student about gaps.
