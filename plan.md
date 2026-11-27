# Plan: Dino Story Studio

A web app where anyone can write a story idea, have AI turn it into a picture book for a chosen age group, edit it page by page,
choose a size and text layout, get a picture on every page, add covers, extra pages and playful text effects, and download a print-ready PDF.
It does for the public what the owner does today with Claude Code skills in ReadTheStupidText and print-the-damn-book.
The project exists mainly for the owner's hands-on AI engineering experience: Python agents, evals for every agent, model choice measured by evals, and embeddings with vector search.

## Status

| Slice | Title | Status | PR | Issue | File |
| --- | --- | --- | --- | --- | --- |
| 1 | Spike: Gemini vs OpenAI pictures, quality and cost | planned | | | [01](docs/plan/slices/01-picture-service-spike.md) |
| 2 | Investigate: printing and posting 1-5 copies to a customer | planned | | | [02](docs/plan/slices/02-print-and-ship-options.md) |
| 3 | First end-to-end: sign in, write an idea, get a story | planned | | | [03](docs/plan/slices/03-skeleton-idea-to-story.md) |
| 4 | Story evals and the writer's model choice | planned | | | [04](docs/plan/slices/04-story-evals-and-writer-model.md) |
| 5 | "Nobody gets hurt" safety check, with its own evals | planned | | | [05](docs/plan/slices/05-safety-check.md) |
| 6 | Page editor: edit, chat with AI per page, add, remove, move | planned | | | [06](docs/plan/slices/06-page-editor.md) |
| 7 | Book size, text placement and the PDF service | planned | | | [07](docs/plan/slices/07-book-format-and-pdf.md) |
| 8 | The illustrator: cast, reference sheets, a picture per page | planned | | | [08](docs/plan/slices/08-illustration-pipeline.md) |
| 9 | Front and back cover, and extra pages | planned | | | [09](docs/plan/slices/09-covers-and-extra-pages.md) |
| 10 | Text effects: a colour per speaker and playful words | planned | | | [10](docs/plan/slices/10-text-effects.md) |
| 11 | Credits, free starting credits and Stripe payments | planned | | | [11](docs/plan/slices/11-credits-and-payments.md) |
| 12 | Public-domain starter tales with vector search | planned | | | [12](docs/plan/slices/12-starter-tales-vector-search.md) |
| 13 | Model choice for every agent, measured, and cost per book | planned | | | [13](docs/plan/slices/13-model-selection-report.md) |
| 14 | The web shell: theme, header, step bar, credit pill, Buy credits pop-up, responsive rules | planned | | | [14](docs/plan/slices/14-web-shell-and-theme.md) |
| 15 | Account: sign in, welcome, account, download my data, delete account | planned | | | [15](docs/plan/slices/15-account-pages.md) |
| 16 | Who it's for, and the story helper that shapes a thin idea | planned | | | [16](docs/plan/slices/16-audience-and-story-helper.md) |
| 17 | Preview on the web, free preview PDF, print-quality PDF for credits | planned | | | [17](docs/plan/slices/17-preview-and-download.md) |
| 18 | Cast: pick a style, check everyone in words, draw each one at a time | planned | | | [18](docs/plan/slices/18-cast.md) |

Order: 1, 2, 3 and 14 can start together (Slice 3's screens wait for 14). After 3 and 14: 4, 6, 7, 12, 15, 16 and 18. Then 5, 8 and 10; then 9 and 11; 17 after 9 and 11; 13 last.
The book steps, in the order the parent sees them: 1 Story (3, 16, 6), 2 Cast (18), 3 Size & layout (7), 4 Pictures (8), 5 Text effects (10), 6 Front cover and 7 Back cover and 8 Extra pages (9), 9 Preview & download (17). Credits and the Buy credits pop-up: 11 and 14. Sign-in, Welcome, Account: 3 and 15.

Notes: _(none yet; `docs/plan/notes/YYYY-MM-DD-<slug>.md`, one line each)_

## Decisions (owner, 2026-11-26)

1. ~~Anyone can sign up with email or Google.~~ (Superseded 2026-11-27, the mock supersedes this: anyone can sign up with Google, Apple or Facebook; see Slice 3 and Slice 15.) Writing and editing are free; pictures and the finished book cost credits bought by card (Stripe). New accounts get free starting credits, enough for a small book with draft-quality pictures.
2. Picture service: decided by a spike comparing Gemini and OpenAI on quality and cost (Slice 1).
3. Runs on Cloudflare, released only by the merge pipeline. The C# book builder from print-the-damn-book is reused as a separate PDF service.
4. First version: the whole flow to a print-ready PDF (A5 for home, or the KDP 8.5in square), with covers, extra pages, text effects and the safety check. Plus a look at how to print and post 1-5 copies (Slice 2).
5. Audio, translation, export to the Dino app and KDP upload come later.
6. Backend and all agents in Python, with evals, measured model choice per agent, and embeddings with vector search where there is a real use.

Accepted defaults: age groups 2-4 (up to 12 pages, about 15 words a page), 4-6 (15, about 30), 6-9 (18, about 60); public-domain tales offered as optional starting points.

## Decisions (owner, 2026-11-27): the approved designs

The designs in `docs/design/` (see its README; brand direction A, sign-in picture 6) are the source of truth for layout, copy and behaviour; where they and an older line here disagree, the design wins. Each slice has a `#### Design` subsection naming its boards. The rules that hold on every screen:

1. Brand tokens, fonts and the responsive sizes live in Slice 14 and one theme file. The web front end is built first against a mock API; real calls are wired slice by slice.
2. The book has nine steps, always reopenable from the step bar and never locked after approval: Story, Cast, Size & layout, Pictures, Text effects, Front cover, Back cover, Extra pages, Preview & download.
3. The credit balance shows on every screen; every paid button shows its cost; running out opens the Buy credits pop-up over the current screen. Prices, credit amounts and the refund rule stay placeholders (`[PRICE]`, `[N]`, `[CREDITS]`, `[REFUND POLICY FOR UNUSED CREDITS]`).
4. Pictures are always cheap drafts (no quality picker, no accept step); a click opens a chat with "Change this picture" or "Draw a new picture"; the full-quality redraw happens only when the print PDF is bought.
5. Covers and extra pages are never drawn automatically; an editable suggested prompt waits for Draw.
6. Size & layout comes before pictures; "words on the picture page" (30% band) only for A5 portrait and only when the text fits.
7. Preview & download has a free preview PDF and a paid print-quality PDF, and no rebuild button.
8. Phone portrait: only My books and the account screens work; every book screen says "Turn your phone sideways".

## Outstanding work

- **Not yet tried on a device:** 0 checks across 0 slices, in [docs/plan/unverified.md](docs/plan/unverified.md).
- **Open point:** the PDF service stays C# (Decision 3) while everything else is Python. If the owner wants it in Python too, that is a new slice.

## Out of Scope

- Read-aloud audio and voices, translations, export to the Dino Reader app, uploading to Amazon KDP (later, Decision 5).
- Ordering printed copies from the app (only investigated in Slice 2 for now).
- Mobile apps; this is a website.
- The owner's own store books, "More books" and Dino QR pages.
