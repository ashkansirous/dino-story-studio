### Slice 5 — "Nobody gets hurt" safety check, with its own evals

*Planned.* **Starts after:** Slice 4.

**Touches:** `api/src/studio/agents/safety/` (new), `evals/datasets/safety/` (new), `evals/safety/` (new), `web/src/features/story/`

**Why.** The owner's books follow one hard rule: nobody gets hurt (ReadTheStupidText `.claude/skills/create-dino-story/SKILL.md:12`).
A public app also gets stories written by strangers, so every story, every page edit and every picture prompt is checked before it is used.

**Decisions**
- The check runs on the writer's output, on every saved page edit or AI chat change, and on every picture prompt (Slice 8 calls it).
- A failed check never silently rewrites the parent's text: it shows what is wrong on which page and offers an AI fix the parent can accept or not.
- Two levels: "not for children" (blocked: violence, sexual content, real people, hate) and "house rule" (someone gets hurt, scary for the age group), which the parent may override with a warning shown.

**Work**
- [ ] Safety agent: returns per-page findings as structured output (level, page, reason, suggested fix). Start from the dino-story-reviewer rules.
- [ ] Hook it into story generation and page saves; show findings on the page editor with "Fix with AI" and "Keep anyway" (house-rule level only).
- [ ] Eval dataset: 60 labelled pages (safe, house-rule break, blocked), including near misses (a wolf that is only grumpy, a fall with no hurt).
      Measure misses and false alarms separately; a missed "blocked" page is a failing run.
- [ ] Pick the safety model from the eval (cheapest model with zero misses on "blocked"); record in `evals/README.md`.

**Verification**
- Eval run shows zero missed "blocked" pages for the chosen model.
- Playwright: a story with a hurt character shows the finding on the right page, and "Fix with AI" changes only that page.
