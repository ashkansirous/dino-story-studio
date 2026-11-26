### Slice 3 — First end-to-end: sign in, write an idea, get a story

*Planned.*

**Touches:** `api/` (new), `web/` (new), `worker/` (new), `wrangler.jsonc` (new), `.github/workflows/` (new), `CLAUDE.md`, `README.md`

**Why.** The smallest thing that works for a real person: sign in, type a story idea (one line, or page by page), pick an age group and a page count,
and see a proper story split into pages, written by Claude. Everything later builds on this.

**Decisions locked with the owner (2026-11-26)**
- Backend and every AI agent in Python ("it needs to be implemented with python"). The goal is hands-on AI engineering.
- Hosted on Cloudflare and released only by the merge pipeline (Decision 3).
- Public sign-up with email or Google (Decision 1).
- Age groups and limits (accepted default):

  | Age group | Most pages | Words per page |
  | --- | --- | --- |
  | 2-4 | 12 | about 15 |
  | 4-6 | 15 | about 30 |
  | 6-9 | 18 | about 60 |

  The parent picks the page count from a list up to the age group's limit.
- The writer starts from ReadTheStupidText `.claude/agents/dino-story-writer.md` and `.claude/skills/dino-storytelling/SKILL.md` (one action per page, read-aloud rhythm), rewritten for strangers' stories.

**Decided by this planning session, from the premise check (2026-11-26)**
- Python FastAPI runs in a Cloudflare Container; a small TypeScript Worker serves the React site and forwards `/api/*` to the container
  (Containers are reached only through a Worker and a Durable Object: developers.cloudflare.com/containers/platform-details/architecture).
  Python Workers were not chosen: the docs call package support "early stages" and do not list the `anthropic` SDK.
- Release: GitHub Actions runs `wrangler deploy` on merge to `main` (Workers Builds preview uploads do not update containers: developers.cloudflare.com/workers/ci-cd/builds/). Never from a shell.
- Data: Postgres on Neon with pgvector (Slice 12 needs vector search; native Python drivers). Files in R2 through its S3 API.
- Sign-in: a hosted service with email and Google whose token the Python API checks. Pick between Clerk and Supabase Auth in the first task by reading their docs and free tiers (`library-docs`).
- Claude calls use the `anthropic` Python SDK with structured output (`client.messages.parse` with a Pydantic model); every call records model, tokens, cost and time per book.

**Work**
- [ ] Repo layout: `api/` (Python 3.13, `uv`, FastAPI, pytest, ruff, mypy), `web/` (React + TypeScript + Vite, Vitest), `worker/` (TypeScript Worker + container binding).
      Load `stack-conventions`, `scaffold-react-app` and `device-test-pipeline` first.
- [ ] First deploy check: a hello-world FastAPI container behind the Worker, released by the GitHub Actions workflow, `GET /api/health` answers on the live URL.
- [ ] Sign-in (email and Google), user row in Postgres.
- [ ] "New book" screen: idea box (with a hint showing both styles: one line, or one line per page), age group, page count. A loading state with Cancel while the story is written.
- [ ] Writer agent (`api/src/studio/agents/writer/`): idea + age group + page count in, title + pages (text, one action each) out, as structured output.
      Prompts live in `api/src/studio/prompts/` as files, never inline strings.
- [ ] Story view: the pages in order. Books are saved; "My books" lists them.
- [ ] LLM call log table (model, tokens in/out, cost, time, book, purpose) from day one; it feeds credits (Slice 11) and model choice (Slice 13).
- [ ] Analytics and logs count only real users on the production build (`production-only-analytics`).
- [ ] Playwright e2e for the whole flow against a local run, with the Claude call replaced by a recorded answer.

**Verification**
- On the live URL: sign in with Google, write a one-line idea for ages 4-6 with 10 pages, and get 10 pages back.
- `uv run pytest`, `npm test` and the Playwright flow pass in CI.
