---
name: presentation
description: Create an academic presentation from course content — reads any source type, checks subject context, enters plan mode to define structure and exposition time, generates a slide brief for approval, then produces a styled HTML file ready to export as PDF.
argument-hint: "[topic, files, or URLs]"
allowed-tools: Read, WebFetch, Bash, Write, EnterPlanMode
---

Generate a presentation for a student from academic content. Follow this workflow precisely.

## Setup

Run workflow-core steps W1, W2, W3, W4.
If W2 returns no content, ask: "¿Sobre qué tema es la presentación? Podés pegarme el contenido, compartir archivos, o decirme el tema y lo genero desde tu programa de la materia."
Use PLAN_MODE from W4 for the plan step below.

## Step 1 — Enter plan mode

Present this plan to the student:

```
## Plan de presentación

**Contenido detectado:**
- [list sources and identified topics]

**Tema principal:** [inferred from content]

**Opciones a confirmar:**

1. ¿Tiempo de exposición?
   - [ ] 5 minutos (~4 slides)
   - [ ] 10 minutos (~7 slides)
   - [ ] 15 minutos (~10 slides)
   - [ ] 20+ minutos (~14 slides)
   - [ ] Otro: ___ minutos

2. ¿Hay consigna o lineamientos del profesor?
   - [ ] Sí → [pegar consigna aquí]
   - [ ] No

3. ¿Audiencia?
   - [ ] Profesor (formal, técnico)
   - [ ] Compañeros (accesible, con ejemplos)
   - [ ] Exposición oral (puntos clave, sin texto denso)

4. ¿Notas del orador?
   - [ ] Sí
   - [ ] No

5. ¿Idioma?
   - [ ] Español
   - [ ] Inglés
   - [ ] Mismo que el material
```

If a professor assignment was provided, verify the planned presentation covers all required points before proceeding. Flag any requirement not addressed.

Wait for student approval before proceeding.

## Step 5 — Calculate slide count

Use exposition time to determine slide count:
- Formula: 1 slide per 1.5 minutes of exposition
- Reserve: 1 slide for title, 1 for index, 1 for conclusion
- Content slides: total - 3
- Example: 10 min → 7 total → 4 content slides

## Step 6 — Generate slide brief

Before writing the full presentation, produce a brief: one line per slide showing title and main point.

Format:
```
## Brief — [Topic Name]

| # | Título | Punto principal |
|---|--------|-----------------|
| 1 | Título | [topic + subject name] |
| 2 | Índice | Estructura de la presentación |
| 3 | [Section] | [key idea] |
| … | … | … |
| N | Conclusión | [3 main takeaways] |
```

Ask: "¿Este brief te parece bien o querés ajustar alguna slide antes de generar la presentación completa?"

Wait for confirmation.

## Step 7 — Generate HTML presentation

Write the full presentation as a single styled HTML file.

Save as: `presentacion-{kebab-case-topic}.html` in the current directory.

HTML structure:

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Topic]</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: 'Segoe UI', system-ui, sans-serif; background: #1a1a2e; color: #eee; }

    .slide {
      width: 100vw; min-height: 100vh;
      display: flex; flex-direction: column; justify-content: center;
      padding: 4rem 6rem;
      border-bottom: 2px solid #2a2a4e;
      page-break-after: always;
    }

    .slide.cover { background: #16213e; text-align: center; align-items: center; }
    .slide.cover h1 { font-size: 3rem; color: #e94560; margin-bottom: 1rem; }
    .slide.cover .subtitle { font-size: 1.2rem; color: #a8b2d8; }

    .slide.index { background: #0f3460; }
    .slide.content { background: #1a1a2e; }
    .slide.conclusion { background: #16213e; border-left: 6px solid #e94560; }

    h2 { font-size: 2rem; color: #e94560; margin-bottom: 2rem; border-bottom: 1px solid #2a2a4e; padding-bottom: 0.5rem; }
    ul { list-style: none; padding: 0; }
    ul li { padding: 0.6rem 0; font-size: 1.15rem; border-bottom: 1px solid #2a2a4e22; }
    ul li::before { content: "▸ "; color: #e94560; }

    .slide-number { position: fixed; bottom: 1rem; right: 2rem; color: #444; font-size: 0.85rem; }
    .speaker-notes { margin-top: 2rem; padding: 1rem; background: #0f3460; border-left: 4px solid #e94560; font-size: 0.9rem; color: #a8b2d8; border-radius: 4px; }
    .speaker-notes::before { content: "🗣 Nota: "; font-weight: bold; color: #e94560; }

    @media print {
      body { background: white; color: black; }
      .slide { page-break-after: always; background: white; min-height: 100vh; }
      .slide.cover { background: #f8f9fa; }
      h2 { color: #c0392b; }
      ul li::before { color: #c0392b; }
      .speaker-notes { display: none; }
    }
  </style>
</head>
<body>

  <!-- SLIDE 1: Cover -->
  <div class="slide cover">
    <h1>[Topic Title]</h1>
    <p class="subtitle">[Subject Name] · [Student Name placeholder]</p>
  </div>

  <!-- SLIDE 2: Index -->
  <div class="slide index">
    <h2>Contenido</h2>
    <ul>
      [index items]
    </ul>
  </div>

  <!-- Content slides -->
  <div class="slide content">
    <h2>[Section Title]</h2>
    <ul>
      <li>[Point 1]</li>
      <li>[Point 2]</li>
      <li>[Point 3]</li>
    </ul>
    [if speaker notes requested:]
    <div class="speaker-notes">[what to say on this slide]</div>
  </div>

  <!-- SLIDE N: Conclusion -->
  <div class="slide conclusion">
    <h2>Conclusión</h2>
    <ul>
      <li>[Takeaway 1]</li>
      <li>[Takeaway 2]</li>
      <li>[Takeaway 3]</li>
    </ul>
  </div>

</body>
</html>
```

Rules for content:
- Max 5 bullets per slide — split into two slides if needed
- Bullets are phrases, not full sentences
- Use the course's own terminology from the material
- Never fabricate data or examples not in the source
- Flag missing content: add a slide with `⚠️ [Tema X]: completar manualmente`

## Step 8 — Confirm and offer next steps

After saving the file, tell the student:

```
✅ Presentación generada: presentacion-{topic}.html

Próximos pasos:
- 📄 Exportar a PDF: /u-student:export presentacion-{topic}.html
- 🗣 Simular defensa: /u-student:defense presentacion-{topic}.html
```
