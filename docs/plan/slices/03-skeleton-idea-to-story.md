### Slice 3 — First end-to-end: sign in, write an idea, get a story

*Planned.* **Starts after:** Slice 14 for its screens (the `api/`, `worker/` and deploy work can start first).

**Touches:** `api/` (new), `worker/` (new), `web/src/features/books/` (new), `web/src/features/new-book/` (new), `wrangler.jsonc` (new), `.github/workflows/` (new), `CLAUDE.md`, `README.md`

**Why.** The smallest thing that works for a real person: sign in, type a story idea (one line, or page by page), pick an age group and a page count,
and see a proper story split into pages, written by Claude. Everything later builds on this.

**Decisions locked with the owner (2026-11-26)**
- Backend and every AI agent in Python ("it needs to be implemented with python"). The goal is hands-on AI engineering.
- Hosted on Cloudflare and released only by the merge pipeline (Decision 3).
- ~~Public sign-up with email or Google (Decision 1).~~ Superseded 2026-11-27 (the mock supersedes this): public sign-up with Google, Apple or Facebook, one tap, no email and password (`docs/design/account/Main.dc.html`; owner brief "Sign in (Google, Apple, Facebook)").
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
- Sign-in: a hosted service whose token the Python API checks. Pick between Clerk and Supabase Auth in the first task by reading their docs and free tiers (`library-docs`); it must offer Google, Apple and Facebook (changed 2026-11-27 from "email and Google"). Each provider needs its own app registration; the first task lists them from the vendor docs.
- Claude calls use the `anthropic` Python SDK with structured output (`client.messages.parse` with a Pydantic model); every call records model, tokens, cost and time per book.

**Decided by the owner's design (2026-11-27)**
- The screens in this slice are My books and "New book, step 1 of 2". They are built with Slice 14's shell and, until the API is ready, its mock API.
- "Writing and editing are free. Pictures and the print PDF use credits." is the line under My books; the credit pill shows on every screen after sign-in.
- Step 2 of 2 ("Who it's for") and "Shape your story" are Slice 16. Until Slice 16 is built, "Next: who it's for" on step 1 writes the story straight away; Slice 16 puts the two screens in front of the writer.

#### Design (App Design mock, sections 1 and 2 (boards 1-2; devices 14-15, 27-28, 40-41, 53-54), added 2026-11-27)

Two frames at desktop, each at four device sizes. The mock agrees with the plan about the content of the idea screen (idea box, age group, page count) and changes the sign-in methods (above) and the screen order (Slice 16).

- **My books** (`app/Main.dc.html`; works in every size including phone portrait): the app header (Slice 14); main max 1200px, padding 40px clamp(16px,4vw,40px) 64px. h1 "My books" (Fredoka 600 40px; 26px under 600px, 22px in phone landscape); "Writing and editing are free. Pictures and the print PDF use credits." (17px, `--muted`; hidden in phone landscape); button "New book" (`--primary`, plus icon). A green notice card for new accounts (fill #F1FAF2, border #C8E8CB, a gift icon 44px): "Your starting credits are ready", "Enough for one 12-page book with draft-quality pictures.", link "Start your first book →" (shown until the first book exists; the wording follows Slice 11's grant). A grid of book cards (`.bkgrid`, auto-fill, min 180px in phone landscape) each with a picture area (height 110px in phone landscape), the title (e.g. "The Hen Who Shared Her Bread"), "Ages 4–6 · 12 pages", the edited date ("Edited today", "Yesterday", "3 Nov") and a status line: "Story ready · add pictures", "Pictures 6 of 10", "PDF ready · 8.5in square". A card opens the book at the step its status names (the mock links its three cards to Story, Pictures and Extra pages). The last card is "Start a new book" / "From your own idea".
- **New book, step 1 of 2** (`app/NewBook.dc.html`): "← My books" in the header; "New book · step 1 of 2"; h1 "What's your story about?"; "Write one line, or one line per page. It's your story — we help you shape it, then write the pages."; label "Your idea" with a text area (placeholder: "Write one line, like: A dino who is scared of the dark" then a blank line, "Or one line per page, like:", "1. Rex wakes up", "2. He can’t find his hat…"); "Age group" as three radio cards (min-height 88px, radius 16px, padding 16px): "Ages 2–4" "Up to 12 pages · ~15 words a page", "Ages 4–6" "Up to 15 pages · ~30 words a page", "Ages 6–9" "Up to 18 pages · ~60 words a page" (selected: 2px `--primary` on `--primary-tint`; the hint line is hidden in phone landscape); label "Number of pages" with a select from 6 up to the age group's limit ("6 pages" … "12 pages"), under it "Up to 12 pages for ages 2–4"; the link-button "Next: who it's for" and "Free — no credits used".
- Responsive and phone-portrait behaviour as Slice 14: My books works on a phone held upright; New book shows "Turn your phone sideways".
- New strings: all quoted lines above.

**Work**
- [ ] Repo layout: `api/` (Python 3.13, `uv`, FastAPI, pytest, ruff, mypy) and `worker/` (TypeScript Worker + container binding). The `web/` scaffold is Slice 14's. Load `stack-conventions` and `device-test-pipeline` first.
- [ ] First deploy check: a hello-world FastAPI container behind the Worker, released by the GitHub Actions workflow, `GET /api/health` answers on the live URL.
- [ ] Sign-in (Google, Apple, Facebook), user row in Postgres. The Welcome screen, account page and deletion are Slice 15.
- [ ] "My books" screen as drawn, with the book cards, their status line, the new-account notice and the "Start a new book" card; books are saved and listed.
- [ ] "New book, step 1 of 2" as drawn: idea box (hint showing both styles: one line, or one line per page), age group, page count. A loading state with Cancel while the story is written.
- [ ] Writer agent (`api/src/studio/agents/writer/`): idea + age group + page count in, title + pages (text, one action each) out, as structured output.
      Prompts live in `api/src/studio/prompts/` as files, never inline strings.
- [ ] Story view: the pages in order (Slice 6 turns it into the editor).
- [ ] LLM call log table (model, tokens in/out, cost, time, book, purpose) from day one; it feeds credits (Slice 11) and model choice (Slice 13).
- [ ] Analytics and logs count only real users on the production build (`production-only-analytics`).
- [ ] Playwright e2e for the whole flow against a local run, with the Claude call replaced by a recorded answer.

**Verification**
- On the live URL: sign in with Google, write a one-line idea for ages 4-6 with 10 pages, and get 10 pages back.
- `uv run pytest`, `npm test` and the Playwright flow pass in CI.
- Screenshots of My books and New book at the five sizes are in the PR.

**Plan changed to match the mock (2026-11-27):**
- Sign-in: "email or Google" becomes Google, Apple and Facebook (the mock supersedes this; plan.md Decision 1 is marked the same way).
- The one New book screen becomes "step 1 of 2" with the link "Next: who it's for" (Slice 16 owns step 2 and the chat).
- My books gets the drawn cards, status lines and the starting-credits notice (the plan only said "My books lists them").
- `web/` scaffold moved to Slice 14.
