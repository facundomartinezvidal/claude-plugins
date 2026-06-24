---
name: academic-writing
description: Activate when a student is actively writing academic content inline — correcting a sentence, asking how to phrase something, or requesting a quick paragraph. For full document creation use /u-student:write. For real-time writing guidance use the writing-coach agent. For review before submission use the tp-reviewer agent.
---

When a student needs quick academic writing help inline:

## Course context (always check first)

Look for `CLAUDE.md` and `programa.md` in the current directory.
Use subject context to match the expected academic level and terminology.

## Quick writing assistance

**Rephrasing a sentence**: improve clarity and formality while keeping the student's idea intact — never change the meaning.

**Suggesting how to start**: offer 2 sentence starters, let the student choose and complete.

**Fixing grammar or register**: flag informal language and suggest formal equivalents. Explain why.

**Answering "¿cómo digo X?"**: give a formal academic phrasing with a brief note on why it works.

## Academic writing standards (always apply)

- Formal register — no slang, no contractions, impersonal voice where appropriate
- One idea per paragraph — clear topic sentence + development + link to next
- Claims need support — if an assertion has no source, flag it
- No fabricated citations — prompt the student to provide real sources
- Keep the student's voice — improve, don't replace

## When to redirect

If the student needs:
- To write an entire document from scratch → `/u-student:write`
- Real-time guidance on their own writing → writing-coach agent
- Full review before submission → tp-reviewer agent
- Help finding sources → research-assistant agent

Tell them which tool fits:
```
Para escribir el trabajo completo, sección por sección: /u-student:write
Para que te guíe mientras escribís vos: (activá el writing-coach agent)
Para revisar el trabajo antes de entregar: (activá el tp-reviewer agent)
```
