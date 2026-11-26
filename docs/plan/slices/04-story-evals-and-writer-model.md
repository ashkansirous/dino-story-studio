### Slice 4 — Story evals and the writer's model choice

*Planned.* **Starts after:** Slice 3.

**Touches:** `evals/` (new), `evals/datasets/story/` (new), `api/src/studio/agents/writer/`, `.github/workflows/evals.yml` (new)

**Why.** The owner's goal for this project is hands-on AI engineering: "I need ai evals, proper model selection for agents"
(owner, 2026-11-26). The story writer is the first agent, so it gets the first eval suite and the first measured model choice.

**Decisions**
- Every agent has an eval suite before its model is chosen, and a model change must be backed by an eval run (rule for `CLAUDE.md`).
- Evals run with one Python tool picked in the first task (`library-docs`: Inspect AI, promptfoo or DeepEval), the same tool for every later agent.
- Two kinds of check: code checks (page count, words per page, one action per page, no page over the age group's limit) and
  model-graded checks (story makes sense, keeps the parent's idea, reads aloud well, fits the age group), each judge prompt with its own small set of human-labelled examples
  so the judge itself is checked.

**Work**
- [ ] Dataset: 30 story ideas across the three age groups and both input styles (one-line idea, page-by-page outline), plus 10 public-domain tales;
      stored as JSONL in `evals/datasets/story/`. Include 5 tricky ones (very long outline, outline asking for more pages than the age group allows, non-English input).
- [ ] Code checks for the age-group rules from Slice 3 (`api/src/studio/books/age_groups.py`).
- [ ] Model-graded checks with a rubric adapted from ReadTheStupidText `.claude/agents/dino-story-reviewer.md` (text mode).
- [ ] Label 20 writer outputs by hand (owner or session, recorded who) and measure how often the judge agrees; keep the judge only above 80% agreement.
- [ ] Run the writer on at least three models (the current Opus, Sonnet and Haiku, IDs from the Anthropic docs that day) and record pass rate, cost per story and time per story.
- [ ] Pick the writer's model from the numbers, write the table and the pick into `evals/README.md`, and set it in the writer's config.
- [ ] CI: the code checks run on every PR that touches `api/src/studio/agents/` or `api/src/studio/prompts/`; the model-graded run is a manual workflow (it costs money).
      Eval calls are tagged as evals so they never count as real users (`production-only-analytics`).

**Verification**
- `uv run` of the eval command prints the per-model table; the chosen model's pass rate is in `evals/README.md`.
- A PR that breaks the page-count rule fails the CI eval job.
