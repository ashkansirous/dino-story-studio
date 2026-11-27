### Slice 8 — The illustrator: a draft picture for every page, chat about a picture, and the print-quality redraw

*Planned.* **Starts after:** Slices 1, 7, 14 and 18.

**Touches:** `api/src/studio/agents/illustrator/` (new), `api/src/studio/agents/picture_chat/` (new), `api/src/studio/jobs/` (new), `worker/src/workflows/` (new), `web/src/features/pictures/` (new), `evals/datasets/pictures/` (new), `evals/pictures/` (new)

**Why.** "then the illustrator would take over and generate images per page" (owner, 2026-11-26). The owner's current method keeps characters looking the same
with one reference sheet per character, a line-up, and a place sheet for each setting that comes back, pasted into each page's request
(ReadTheStupidText `.claude/skills/create-dino-story/SKILL.md`, "Who does what" and "3. Gemini setup").
The cast agent, style choice and reference sheets are now Slice 18 (the mock makes Cast its own step before Size & layout); this slice uses the approved sheets.

**Decisions**
- The picture service is the one Slice 1 picks (written here when it is picked).
- Two quality levels: "draft" (cheapest setting, used by free starting credits) and "print" (enough pixels for 300 dpi at the chosen size). The plan left open where each level is used; that is settled 2026-11-27 (the mock supersedes this; owner brief: "Pictures are always cheap drafts: no quality picker and no accept step ... The full-quality redraw happens only when the print PDF is bought"): every page picture on the Pictures screen is a draft. There is no quality picker and no accept step. Print quality is made only when the print PDF is bought (Slice 17): "We redraw every page's current picture in full quality (the same picture, 300 dpi)".
- "The same picture" at full quality means the same prompt, the same reference sheets and the same style as the version the parent ended with (the job keeps them per version), with the highest setting of the service.
- Covers (Slice 9) and extra pages are never drawn automatically; only the story pages are, and only when the Pictures step starts.
- Pictures are made as a background job: one step per picture, each saved to R2 as soon as it is done, so the parent watches them appear and a failure redoes one page, not the book.
  The job runner is chosen in the first task: Cloudflare Workflows (Python is supported, one step per picture: developers.cloudflare.com/workflows/python/) or a job table in Postgres worked by the container.
- Every picture prompt goes through the safety check (Slice 5). Real names never reach the model (Slice 16).
- Earlier versions of a page's picture are kept; going back to one is free.
- A picture is never redrawn by itself: if its cast sheet or the book's size changed, the page shows "for a redraw" and waits for the parent.

#### Design (App Design mock, section 8 "Pictures appearing, credits" (board 8; devices 21, 34, 47, 60), added 2026-11-27)

One desktop board and its four device sizes. The mock changes the plan: pictures are always drafts, a click on a picture opens a chat with two actions, and there is no accept step. The picture figures are simplified illustrations (coloured shapes standing for drawn pages), not UI.

- Book header: "✓ Story", "✓ Cast", "✓ Size & layout", "4 Pictures" current; "Ages 4–6 · 12 pages · A5 portrait".
- Layout "split": the page grid in the main column, the selected page in a side pane (`<aside aria-label="Selected page">`). Tablet portrait side by side; under 700px stacked; phone landscape each pane scrolls alone.
- Main: h1 "Drafts of your pages"; counter "10 of 12 drawn"; "Every page is drawn as a quick, low-cost draft. Click any picture to talk about it, change it or draw it again — as often as you like."; a grid of page thumbnails (aspect ratio follows the chosen size: 1:1.414 for A5, square for the square size; radius 10px; selected: 3px `--primary` border and a soft green shadow; others 1px `--border`). Each is a button (aria-label "Page 3, Drawn") with the page number and a status tag top right: "Drawn" (`--primary-tint`, #1B5E20), "Drawing" (warning colours), "Waiting" (`--soft`, `--muted`; the thumbnail is `--soft`). Link "Next: text effects →". Footer line: "Drafts cost [DRAFT CREDITS] each. Full-quality copies of the pictures you end up with are made only when you order the print PDF."
- Side pane: the big thumbnail (200px wide) and the page text beside it; the line "Draft · version 1"; h2 "Page 2" with the status tag; h3 "Talk about this picture" with a short chat: the parent ("Can the cat look grumpier? And the hen is too small.") and the helper ("Got it. I'd change this picture: a grumpier cat with folded paws, and Hen a bit bigger in the front. Should I keep everything else the same?"); the field "Message about this picture" ("Say what you'd like different…") with a send icon button (aria-label "Send").
- "How to redraw": two radio cards, "Change this picture" "Keep it, add or fix things" and "Draw a new picture" "Start again from the prompt" (selected: 2px `--primary` on `--primary-tint`).
  Change: label "What to change (filled from your chat, edit anything)" with a text area filled by the chat ("Make the cat grumpier, with folded paws. Make Hen a bit bigger in the front. Keep everything else the same."), and the CostButton "Change this picture · [DRAFT CREDITS]".
  New: label "The prompt used for this page (edit anything)" with the full prompt in an editable text area ("Little Red Hen in the farmyard holding up a grain of wheat, asking the Cat and the Dog for help. … Bottom 30% calm and plain for the words."), "Uses:" and the cast and place chips ("Little Red Hen", "The Cat", "The Dog", "The farmyard"), and the CostButton "Draw a new picture · [DRAFT CREDITS]".
  After a redraw: "Earlier versions of this page are kept. Go back to version 1 (free)".
- Not enough credits: the Buy credits pop-up opens over this screen with "Not enough credits" and "Changing this picture needs more credits than you have left. Top up and we’ll draw it straight away." (Slice 14); nothing is drawn.
- New strings: all quoted lines above.

**Work**
- [ ] Illustrator: from the story, the page's prompt (written by a prompt-writing step that uses the approved cast sheets, the style and the layout brief: portrait or square, bottom 30% calm when the words sit on the picture), draw a draft per page; the prompt is saved per page and shown in the editor.
- [ ] Picture-chat agent: the parent talks about one picture; the agent answers and fills "What to change" with a short edit instruction; "Change this picture" sends the current picture plus that instruction, "Draw a new picture" sends the edited prompt alone.
- [ ] Pictures screen as drawn: the grid with live status as the job runs (pictures appear one by one), the side pane with chat, the two redraw modes, version history with free "go back", the cost line. The CostButtons show the credit price from Slice 11 (placeholders until then).
- [ ] The job starts when the Pictures step is opened with no pictures yet and asks first with the total ("Draw 12 drafts · [DRAFT CREDITS] each", the balance, and the Buy credits pop-up if it is short; this confirmation is carried from Slice 11, the mock does not draw it).
- [ ] Picture eval: the Slice 1 vision-judge prompt, plus an embedding check: an image embedding of each character crop compared with its reference sheet, flagging pages below a threshold.
      Measure how well the embedding score agrees with the judge and with the owner's scores on 3 test books.
- [ ] Pick the judge model, the illustrator's prompt-writing model and the picture-chat model from the evals (`evals/README.md`).
- [ ] Print-quality redraw job for Slice 17: takes the current version of every page, redraws it at print quality with the same prompt, sheets and style, saves it to R2, resumable; a failed picture is retried alone.
- [ ] The PDF from Slice 7 uses the real pictures.

**Verification**
- A 12-page test book gets 12 draft pictures; killing the job halfway and resuming redraws only the missing pages.
- "Change this picture" keeps the earlier version and costs one draft; "Go back to version 1" is free.
- A page whose cast sheet changed shows "for a redraw" and is not redrawn until asked.
- Picture eval table with judge/owner agreement in `evals/README.md`.
- Screenshots at the five sizes are in the PR.

**Plan changed to match the mock (2026-11-27):**
- Cast agent, style choice and reference sheets moved to Slice 18 (own step before Size & layout; the plan had a "Cast screen" here).
- "draft" and "print" levels: pictures on this screen are always drafts; no quality picker and no accept step; print only at purchase (Slice 17). The plan's "Redraw this page with an optional note" becomes the chat with "Change this picture" and "Draw a new picture", with the prompt shown and editable.
- Order: this slice now waits for Slice 7 (Size & layout before pictures, kept from the plan) and Slice 18.
- Version history with a free go-back is added (the mock draws it).
