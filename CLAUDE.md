# Dino Story Studio

Web app that turns a parent's story idea into an illustrated, print-ready picture book. Plan: `plan.md` (index) and `docs/plan/slices/`.

## Rules

- The backend and every AI agent are Python (3.13, `uv`). The web front end is React + TypeScript. The PDF service in `pdf/` is C# (.NET 10, QuestPDF), copied from print-the-damn-book.
- Every agent has an eval suite in `evals/` before its model is chosen. A change of model or prompt for an agent comes with an eval run, and its numbers go in `evals/README.md`.
- Prompts are files under `api/src/studio/prompts/`, never inline strings. Model IDs live only in the model config, never in agent code.
- Every Claude or picture call is logged with model, tokens, cost and time; credits and model choice are read from that log.
- Eval, test and CI calls never count as real users in analytics or usage counts.
- Releases happen only through the GitHub Actions workflow on merge to `main`; never `wrangler deploy` from a shell.
- Stories and picture prompts pass the safety check before they are used.
