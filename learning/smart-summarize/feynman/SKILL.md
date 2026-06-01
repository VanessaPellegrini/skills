---
name: summarize-feynman
description: Feynman Technique — explain complex concepts as if teaching a 12-year-old. Use when user wants simple, jargon-free explanation of a complex topic.
---

# Feynman Technique

Can't explain simply = don't understand.

## Steps

1. **Read** source thoroughly
2. **Identify** core concept(s)
3. **Explain** as if teaching a 12-year-old
4. **Detect** remaining jargon or hand-waving
5. **Simplify** those parts (go back to source if needed)
6. **Document** gaps — indicate areas for deeper study

## Output Format

```markdown
# [Concept Name]

## What is it?
[Simple explanation. No jargon. 12-year-old should understand.]

## How does it work?
[Step-by-step in plain language. Use analogies.]

## Why does it matter?
[Real-world relevance]

## Example
[Concrete illustration]

## Gaps Found
[What was hard to simplify — areas for deeper study]
```

Frontmatter + Source Processing + Language: see `auto/SKILL.md`.

## Rules

- NO jargon without immediate explanation
- Use everyday analogies
- 3+ sentences of setup = too complex, simplify
- Every technical term needs parenthesized plain-language equivalent
- "Gaps Found" mandatory
