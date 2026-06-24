---
name: export
description: Export an academic document or presentation to PDF — supports .html, .md, and .docx files
argument-hint: "[file.html | file.md | file.docx]"
allowed-tools: Bash
---

Export a document or presentation to PDF.

## Step 1 — Validate input

If no file path provided, list exportable files in current directory:
```bash
ls *.html *.md *.docx 2>/dev/null
```
If multiple found, ask which one to export.
If none found, ask: "¿Cuál es el archivo que querés exportar a PDF?"

## Step 2 — Detect file type and export

### HTML files (.html)

Detect Chrome/Chromium:
```bash
ls "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" 2>/dev/null || \
which google-chrome 2>/dev/null || which chromium 2>/dev/null
```

If found:
```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --disable-gpu --print-to-pdf="{output}.pdf" \
  --print-to-pdf-no-header "{input}.html" 2>/dev/null || \
google-chrome --headless --disable-gpu \
  --print-to-pdf="{output}.pdf" "{input}.html" 2>/dev/null
```

If not found, detect wkhtmltopdf:
```bash
which wkhtmltopdf 2>/dev/null
```
If found:
```bash
wkhtmltopdf --page-size A4 --orientation Landscape "{input}.html" "{output}.pdf"
```

### Markdown files (.md)

Check for pandoc:
```bash
which pandoc 2>/dev/null
```
If found:
```bash
pandoc "{input}.md" -o "{output}.pdf" --pdf-engine=wkhtmltopdf 2>/dev/null || \
pandoc "{input}.md" -o "{output}.pdf"
```

If pandoc not found, convert to HTML first then use Chrome:
```bash
# Use pandoc or python markdown to HTML, then Chrome to PDF
TMPFILE=$(mktemp /tmp/u-student-export-XXXXXX.html) && \
python3 -c "
import sys, markdown, pathlib
content = pathlib.Path(sys.argv[1]).read_text()
html = markdown.markdown(content, extensions=['tables','fenced_code'])
print(f'<html><head><meta charset=\"utf-8\"><style>body{{font-family:Georgia,serif;max-width:800px;margin:40px auto;padding:0 20px;line-height:1.6}}h1,h2,h3{{color:#2c3e50}}table{{border-collapse:collapse;width:100%}}td,th{{border:1px solid #ddd;padding:8px}}</style></head><body>{html}</body></html>')
" "{input}.md" > "$TMPFILE" 2>/dev/null && \
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --disable-gpu --print-to-pdf="{output}.pdf" \
  "$TMPFILE" 2>/dev/null; rm -f "$TMPFILE"
```

### Word files (.docx)

Check for pandoc:
```bash
which pandoc 2>/dev/null
```
If found:
```bash
pandoc "{input}.docx" -o "{output}.pdf" 2>/dev/null
```

If pandoc not found:
```bash
# Try LibreOffice headless
which libreoffice 2>/dev/null || which soffice 2>/dev/null
libreoffice --headless --convert-to pdf "{input}.docx" --outdir "$(dirname {input})" 2>/dev/null
```

## Step 3 — Handle missing tools

If no tool available for the detected file type, tell the student:

```
No encontré herramienta para exportar automáticamente.

Para instalar:
  brew install pandoc          # convierte md y docx
  brew install wkhtmltopdf     # convierte html

Método manual:
  - HTML/MD: abrí en Chrome → Cmd+P → "Guardar como PDF" → Landscape, sin márgenes
  - DOCX: abrí en Word o Google Docs → Archivo → Descargar como PDF
```

## Step 4 — Confirm result

If export succeeded:
```
✅ Exportado: {output}.pdf
```

If failed, show the exact error and suggest the manual method.
