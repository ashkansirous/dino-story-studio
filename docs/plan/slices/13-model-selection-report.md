### Slice 13 — Model choice for every agent, measured, and a cost per book

*Planned.* **Starts after:** Slices 5, 6, 8, 10, 12, 16 and 18 (16 and 18 are the story helper and the cast agent and sheet artist that the design adds; Slices 8 and 11 add the picture-chat agent and the credit prices it feeds).

**Touches:** `evals/`, `evals/README.md`, `api/src/studio/config/models.py` (new), `docs/plan/notes/` (new)

**Why.** "proper model selection for agents" (owner, 2026-11-26). Each earlier slice picks one agent's model alone; this slice
compares them all together, with cost per finished book, so the choices are consistent and can be re-run when new models come out.

**Work**
- [ ] One command runs every agent's eval suite on a list of candidate models and writes a table: pass rate, cost and time per call, cost per 12-page book.
- [ ] Model choice per agent in one config file, each with the eval run that justifies it (date and numbers).
- [ ] Try the cheaper choices that the per-agent picks did not: prompt caching for the long shared rules, the Batch API for evals, a small model with a fallback to a larger one when a check fails.
- [ ] A note in `docs/plan/notes/` with the full table, what changed and the new cost per book; Slice 11's price list is updated if costs moved.

**Verification**
- The command runs end to end and the config file matches the table.
