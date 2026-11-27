### Slice 16 — Who it's for, and the story helper that shapes a thin idea

*Planned.* **Starts after:** Slices 3 and 14.

**Touches:** `api/src/studio/agents/story_helper/` (new), `api/src/studio/routes/audience.py` (new), `api/src/studio/prompts/`, `web/src/features/new-book/`, `web/src/features/shape/` (new), `evals/datasets/story_helper/` (new), `evals/story_helper/` (new)

**Why.** Owner brief (2026-11-27): "New book (your idea, age, pages) → Who it's for (optional) → Shape your story (story agent talks first, asks questions, suggests options) → Story editor". The plan had a single New book screen that went straight to the writer. The mock puts two screens and a short chat in front of the writer, and says the chat is only used "when the idea is thin".

**Decisions**
- Step 1 of 2 (the idea, age, pages) is Slice 3's screen; this slice adds step 2 of 2, "Who it is for", and "Shape your story".
- After step 2 the story helper reads the idea. "If there's enough to write an original story, we go straight to your pages. If not, we'll talk it through with you first." Enough means the idea gives a character, a problem and a way it could end; if the helper cannot tell, it asks.
- The helper asks one question at a time, each with 2-3 suggested answers, "Other ideas" for more, and a free-text answer. Two questions are "needed" before the pages can be written (in the mock: what goes wrong, how it ends); the others are optional. The question list is the helper's, not a fixed list: the mock's five (why today, what goes wrong, a character's place, how it ends, the feeling) are examples.
- Real names stay out of AI calls. The "Real person" column ("Mia", "Me (Dad)") is kept in our database and shown to the parent only; the writer, the helper and the illustrator see the story names ("Sky the seagull", "Captain Kit"), the role (main, supporting, not in this book) and the description text with the real names replaced by their story names before sending. This makes the mock's line "Names and details stay in your book. They're only used to write and draw your story." true.
- All of this is free (no credits). The "Write my 10 pages" step uses the Slice 3 writer, now given the audience and the helper's answers.
- The helper's prompts are files; its model is chosen by its own eval (this slice); every call goes to the call log.
- Slice 12's "Start from a classic tale" suggestions stay on step 1 under the idea box; the mock does not draw them (see Slice 12).

#### Design (App Design mock, sections 2, 3 and 4 (boards 2-4 and device boards 15-17, 28-30, 41-43, 54-56), added 2026-11-27)

Three frames at desktop, each also at four device sizes. The mock adds screens the plan lacked and changes the plan's flow (see "Plan changed" below). Illustrations: none; the cast list on the right of "Shape your story" is real UI.

- **New book, step 1 of 2** (`app/NewBook.dc.html`, built in Slice 3, listed here for its link): "New book · step 1 of 2", h1 "What's your story about?", "Write one line, or one line per page. It's your story — we help you shape it, then write the pages.", label "Your idea", the age cards, "Number of pages", the link "Next: who it's for", and "Free — no credits used".
- **Who it is for, step 2 of 2** (`app/Audience.dc.html`, route `/books/new/audience`): header "← My books"; "New book · step 2 of 2 ·" with a link "Back to your idea"; h1 "Who is this book for?"; "Optional. Tell us about the reader and the people in their world, and the story helper will shape a story that feels like theirs."; label "Describe them" with a text area (example text in the mock: "My daughter Mia, she's 3. She loves the sea and plays at being a seagull called Sky. I'm always Captain Kit."); "Put them in the story?" as a radio group of three pills (min-height 44px, radius 999px, selected = 2px `--primary` on `--primary-tint`): "Yes, as the main character", "Yes, as a helper", "No, they just read it"; "People and their story names" with the line "Play pretend at home? Use the names you already play with. Add grandparents, a pet or a friend if they should appear."; a table with columns "Real person", "Who they are", "Story name", "Part" (select: "Main", "Supporting", "Not in this book"), a "Remove person" icon button per row (44px), and "+ Add someone"; the line "Names and details stay in your book. They're only used to write and draw your story."; two buttons: "Next: shape the story" and "Skip — it's not for anyone special" (a link-style button). While the helper reads the idea a full-card loading state shows "Reading your idea…", "If there's enough to write an original story, we go straight to your pages. If not, we'll talk it through with you first." and a "Cancel" button.
- **Shape your story** (`app/StoryChat.dc.html`, route `/books/:id/shape`; a "split" layout with the chat in the main column and the cast and answers on the right): book header shows "New book" and "Ages 2–4 · 10 pages · for Mia · Change" (Change links back to step 1); the step bar is shown with "1 Story" current. h1 "Let's shape your story"; "A couple of questions so the story is truly yours. Pick a suggestion or write your own answer."; the helper speaks first: "Here's what I've got so far:" and a list ("The idea: …", "For Mia, 3 · 10 pages, about 15 words each", "Sky is Mia, the main character. Captain Kit (Dad) and Grandma and Grandpa Gull are supporting.") then "Mia gets to be Sky. Let's work out the middle and the ending." Each question shows a small caps label ("WHY TODAY", "WHAT GOES WRONG", "CAPTAIN KIT", "HOW IT ENDS", "THE FEELING") and the question; an answered question shows the answer with a "Change" button; the open question shows "Suggestions" as buttons, an "Other ideas" button, a field "Your own answer" ("Or write your own answer…") and a send icon button (aria-label "Send answer"). When all are answered: "That's a story! Check it on the right, then I'll write your 10 pages."
  Right panel "Your story so far" with progress ("2 of 5" and a bar in `--primary`), "CAST" (names with "Main · Mia", "Supporting · Dad", "Supporting") and a definition list of the parts with the answers ("Not decided yet" in italic `--placeholder` #8a839c until answered; a part that is required carries "· needed"). Button "Write my 10 pages" (full width, `--primary`) is disabled with the label "A few more answers first" until the needed parts are answered, with the hint "I still need what goes wrong and how it ends."; when ready the hint is "Free — you can still change anything on the pages." and it opens the Story editor.
- Phone landscape and tablet: the panes stack under 700px wide and sit side by side from 700px; each pane scrolls on its own in phone landscape.
- New strings: all quoted lines above, in both locales the site ships.

**Work**
- [ ] Step 2 of 2 screen with the table of people (add, remove, edit), the three-way role, and Skip. People are saved with the book.
- [ ] Story helper agent (`api/src/studio/agents/story_helper/`): input idea, age group, page count, audience text and people (story names only); output "ready" or one next question with 2-3 suggestions, plus the summary and the "needed" flags, as structured output. Prompts as files.
- [ ] Shape-your-story screen with the chat, the "Your story so far" panel and the Write button; answers saved with the book so it can be reopened.
- [ ] The Slice 3 writer takes the audience, the people and the helper's answers as extra input; the page limits of Slice 3 still apply.
- [ ] Re-run the Slice 4 writer eval with the audience and answers added to the input (a few dataset rows get them); the page-limit checks must still pass.
- [ ] The name replacement before every AI call (real name to story name) and a unit test that no real name reaches a model call.
- [ ] Safety check (Slice 5) on the idea, the audience text and every free-text answer.
- [ ] Story-helper eval: 30 ideas labelled "enough" or "thin" plus the questions a person would want asked; measure how often "enough" ideas get no questions and how often "thin" ones do; the judge checks each question is one question with usable suggestions. Pick the model and record in `evals/README.md`.
- [ ] Any AI wait over 500 ms shows a loading state with Cancel ("Reading your idea…" is the mock's).

**Verification**
- Playwright: a thin idea goes through step 2 into Shape your story, two needed answers enable "Write my 10 pages", and 10 pages arrive; an "enough" idea skips Shape and goes straight to the pages.
- A test with a person called "Mia" shows only "Sky the seagull" in the recorded model call.
- Story-helper eval table in `evals/README.md`.

**Plan changed to match the mock (2026-11-27):**
- One New book screen going straight to the writer (Slice 3) becomes: step 1, step 2 "Who is this book for?", a story-helper chat only when the idea is thin, then the writer. The mock draws all three screens.
- A new agent (the story helper) with its own eval; Slice 13 compares it with the others.
