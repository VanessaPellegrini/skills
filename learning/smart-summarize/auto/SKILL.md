---
name: smart-summarize
description: Select summarization method by content type. Use when user asks to summarize without specifying method.
---

# Smart Summarize (Auto)

Auto-selects best method by content.

## Method Selection

| Content Type | Method | Why |
|-------------|--------|-----|
| Complex concept / theory | **feynman** | Forces simplification |
| Book / textbook | **adler** | Multi-level analysis |
| Article / blog post | **sq3r** | Structure-aware reading |
| Lecture / video / meeting | **cornell** | Two-column notes |
| Facts / definitions / vocab | **spaced** | Long-term retention |
| Academic paper | **retrieval** | Active recall |
| Multiple related concepts | **concept-map** | Reveals relationships |
| Full synthesis needed | **condense** | All methods combined |

## Workflow

1. Identify content type + user goal
2. Select method from table
3. Announce: "Usando metodo **[X]** porque [reason]"
4. Apply method
5. Deliver summary with frontmatter

## Shared Sections

All sub-skills use these:

### Output Frontmatter

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
method: [method name]
source: [title or URL]
tags: [relevant tags]
---
```

### Source Processing

Extract source text before applying method:

1. **PDF (selectable)**: `pdftotext <file.pdf> -`
2. **Scanned PDF**: `ocrmypdf <file.pdf> /tmp/output.pdf --deskew && pdftotext /tmp/output.pdf -`
3. **Large PDF (>200p)**: `pdftotext -f N -l M <file.pdf> -`
4. **URL**: fetch + extract readable content
5. **YouTube/Video**: fetch transcript
6. **Text/Markdown**: read directly

Extract to plaintext. On failure, ask user for alternative format.

### Language

- Spanish default
- Technical terms in English when standard
- Clear, direct, no filler
