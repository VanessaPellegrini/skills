---
name: summarize-spaced
description: Leitner-system flashcard set for spaced repetition. Use when goal is long-term memorization of facts, definitions, vocabulary, or procedures.
---

# Spaced Repetition (Leitner System)

Ebbinghaus forgetting curve + Leitner system. New=review often, easy=review less.

## The Forgetting Curve

No review: 50% lost/hr, 70%/day, 90%/week. Spaced repetition interrupts at increasing intervals.

## Steps

1. **Extract** key facts, definitions, concepts
2. **Convert** each to Q&A flashcard
3. **Classify** by difficulty:
   - **Hard**: new/complex -> Box 1 (daily)
   - **Medium**: somewhat known -> Box 2 (every 2-3 days)
   - **Easy**: well known -> Box 3 (weekly)
   - **Mastered**: automatic -> Box 4 (monthly)
4. **Schedule** reviews via Leitner progression

## Leitner Rules

- Correct -> next box (less frequent)
- Incorrect -> Box 1 (daily)
- Always review Box 1 daily
- Higher boxes at scheduled interval

## Output Format

```markdown
# [Title] — Spaced Repetition Cards

## Box 1 — Daily (New & Hard)
1. **Q**: ... **A**: ...
2. **Q**: ... **A**: ...

## Box 2 — Every 2-3 Days
1. **Q**: ... **A**: ...

## Box 3 — Weekly
1. **Q**: ... **A**: ...

## Box 4 — Monthly (Mastered)
1. **Q**: ... **A**: ...

## Stats
- Total: [N] | Hard: [N] | Medium: [N] | Easy: [N] | Mastered: [N]
```

## Card Writing Tips

- One concept per card
- Questions: clear, specific
- Answers: concise (1-2 sentences max)
- Include "Why?" cards, not just "What?"
- Use examples when helpful

Frontmatter + Source Processing + Language: see `auto/SKILL.md`.
