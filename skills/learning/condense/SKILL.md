---
name: summarize-condense
description: All summarization methods combined into one comprehensive digest. Use for deepest possible digest of books, papers, or complex topics.
---

# Condense (All Methods)

Full pipeline. All 7 methods combined. For content needing full mastery.

## Pipeline

### Phase 1: Structure (Adler + SQ3R)
1. Adler Inspectional — classify, thesis, outline
2. SQ3R Survey — skim headings, intro, conclusion
3. SQ3R Questions — turn headings into questions

### Phase 2: Capture (Cornell)
4. Cornell Notes — two-column capture

### Phase 3: Understand (Feynman + Concept Map)
5. Feynman — explain core concepts plainly
6. Concept Map — visualize relationships

### Phase 4: Retain (Retrieval + Spaced)
7. Retrieval Practice — free recall, identify gaps
8. Spaced Repetition — generate flashcard set

Total: ~18 min automated.

## Output Format

```markdown
# [Title] — Full Digest

---

## 1. Inspectional Overview (Adler)
[Classification, thesis, structure outline]

---

## 2. Questions (SQ3R)
[Headings turned into questions]

---

## 3. Cornell Notes
| Preguntas | Notas |
|-----------|-------|
| ... | ... |

**Resumen:** [3-5 sentences]

---

## 4. Feynman Explanation
[Core concepts explained simply, gaps found]

---

## 5. Concept Map
[Text-based map of concepts + relationships]

---

## 6. Retrieval Practice
**Remembered:** [from memory]
**Missed:** [gaps]
**Retention:** [X%]

---

## 7. Spaced Repetition Cards
### Box 1 (Daily)
- Q: ... A: ...
### Box 2 (Every 2-3 days)
- Q: ... A: ...
### Box 3 (Weekly)
- Q: ... A: ...

---

## 8. Critical Assessment
[Agreements, disagreements, missing, connections]

---

## 9. Review Schedule
| Date | Method | Focus |
|------|--------|-------|
| Tomorrow | Retrieval | All Box 1 cards |
| Day 3 | Cornell | Re-read notes, cover questions |
| Day 7 | Feynman | Re-explain from memory |
| Day 14 | Concept Map | Rebuild from scratch |
| Day 30 | Spaced | Review Box 1-3 cards |
```

Frontmatter + Source Processing + Language: see `auto/SKILL.md`.
