---
name: summarize-concept-map
description: Concept map — identify key concepts and relationships in structured visual format. Use when content involves multiple interconnected ideas, systems, or frameworks.
---

# Concept Map

Novak's concept mapping: visualize relationships in complex knowledge systems.

## Steps

1. **Identify concepts** — extract 5-15 key concepts
2. **Define each** — one clear sentence per concept
3. **Map relationships** — labeled directional links
4. **Identify hierarchy** — broader vs. specific
5. **Find cross-links** — connections across branches
6. **Create map** — structured text representation

## Link Types

| Link | Meaning |
|------|---------|
| A -> B (includes) | A contains B |
| A -> B (depends on) | A requires B |
| A -> B (produces) | A generates B |
| A -> B (contrasts with) | A opposite to B |
| A -> B (example of) | B specific case of A |
| A -> B (leads to) | A causes B |
| A <-> B (related to) | Bidirectional |

## Output Format

```markdown
# [Title] — Concept Map

## Concepts
- **[Concept 1]**: [One-sentence definition]
- **[Concept 2]**: [One-sentence definition]

## Relationships
- [Concept 1] ->(includes)-> [Concept 2]
- [Concept 1] ->(depends on)-> [Concept 3]

## Map
```
                [Broadest Concept]
                /        |        \
        [Sub 1]    [Sub 2]    [Sub 3]
          |      /    \        |
      [Detail] [D1]  [D2]  [Detail]
                    |
               [Cross-link to Sub 1]
```

## Insights
[Patterns, gaps, surprising connections]
```

Frontmatter + Source Processing + Language: see `auto/SKILL.md`.
