---
name: help
description: Show all available u-student commands, agents, and recommended workflow
allowed-tools: []
---

Show a complete guide of the u-student plugin in Spanish.

Print the following exactly:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  u-student — Asistente académico para Claude Code
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📁 CONFIGURACIÓN

  /u-student:init
    Inicializa el workspace de la materia. Crea la estructura de carpetas,
    el cronograma y un CLAUDE.md que se carga automáticamente en cada sesión.
    Corré esto primero en la carpeta de tu materia.

  /u-student:organize
    Organiza archivos sueltos (PDFs, slides, apuntes) en la estructura estándar
    de content/primer-parcial/ y content/segundo-parcial/.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📚 ESTUDIO

  /u-student:summarize [tema]
    Resume el material de la materia. Podés pedir resumen completo, por parcial
    o por clases específicas. Avisa qué clases del cronograma no tienen material.

  /u-student:study [concepto]
    Sesión guiada para entender un concepto o resolver un problema paso a paso.
    No te da la respuesta directa — te guía para que llegues vos.

  /u-student:explain [concepto]
    Técnica Feynman: vos explicás el tema con tus palabras y Claude identifica
    exactamente qué entendiste bien y qué tenés confuso.

  /u-student:compare [concepto A] [concepto B]
    Tabla comparativa entre dos o más conceptos con síntesis y errores comunes.

  /u-student:mindmap [tema]
    Genera un mapa mental en Mermaid organizado por unidad o parcial.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 EVALUACIÓN

  /u-student:quiz [--parcial 1|2]
    Preguntas interactivas sin respuesta visible. Respondés y recibís feedback
    con ✅/⚠️/❌ y explicación. Muestra puntaje al final.

  /u-student:flashcards [tema]
    Tarjetas de memoria: ve la pregunta → respondés → se revela la respuesta.
    Lleva puntaje y al final sugiere qué repasar.

  /u-student:exam [--parcial 1|2]
    Simulacro completo de parcial: te muestra todas las preguntas, respondés,
    y después corrige con nota estimada y explicación detallada por pregunta.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎨 PRESENTACIONES Y DOCUMENTOS

  /u-student:presentation [tema]
    Crea una presentación HTML. Primero hace un brief de slides para aprobar,
    después genera el HTML completo con notas del orador. Exportable a PDF.

  /u-student:defense [--plan | --simulate]
    --plan: guía escrita de qué decir en cada slide, transiciones y preguntas
            probables del profesor.
    --simulate: simulación oral slide a slide — vos presentás por escrito y
                recibís feedback por slide.

  /u-student:write [tipo]
    Redacta TPs, informes y ensayos sección por sección. Nunca genera todo
    de una vez — aprueba cada sección antes de avanzar.
    Tipos: tp, informe, ensayo, reporte

  /u-student:export [archivo]
    Exporta a PDF. Soporta .html, .md y .docx.
    Usa Chrome, pandoc o wkhtmltopdf según lo que tengas instalado.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🤖 AGENTES (se activan automáticamente)

  tutor            → "no entiendo X", "explicame", "qué es X"
  study-planner    → "tengo el parcial en X días", "armame un plan"
  tp-reviewer      → "revisá mi TP", "corregí el trabajo"
  writing-coach    → "cómo empiezo a redactar", "suena bien esto?"
  research-assistant → "dónde busco fuentes", "esta fuente sirve?"
  exam-analyst     → "tengo parciales viejos", "qué toman siempre"
  progress-tracker → "cuánto me falta", "cómo voy con la materia"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 FLUJO RECOMENDADO

  1. /u-student:init               → configurar la materia
  2. Agregar material a content/   → 01-tema.pdf, 02-tema.pdf...
  3. /u-student:organize           → organizar archivos sueltos
  4. /u-student:summarize          → entender el panorama completo
  5. /u-student:quiz               → evaluar comprensión
  6. /u-student:exam               → simular el parcial
  7. /u-student:presentation       → crear entregables
  8. /u-student:write              → redactar trabajos

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
