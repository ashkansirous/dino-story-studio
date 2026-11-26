### Slice 8 — The illustrator: cast, reference sheets and a picture for every page

*Planned.* **Starts after:** Slices 1 and 7.

**Touches:** `api/src/studio/agents/illustrator/` (new), `api/src/studio/agents/cast/` (new), `api/src/studio/jobs/` (new), `worker/src/workflows/` (new), `web/src/features/pictures/` (new), `evals/datasets/pictures/` (new), `evals/pictures/` (new)

**Why.** "then the illustrator would take over and generate images per page" (owner, 2026-11-26). The owner's current method keeps characters looking the same
with one reference sheet per character, a line-up, and a place sheet for each setting that comes back, pasted into each page's request
(ReadTheStupidText `.claude/skills/create-dino-story/SKILL.md`, "Who does what" and "3. Gemini setup").

**Decisions**
- The picture service is the one Slice 1 picks (written here when it is picked).
- Two quality levels: "draft" (cheapest setting, used by free starting credits) and "print" (enough pixels for 300 dpi at the chosen size).
- Pictures are made as a background job: one step per picture, each saved to R2 as soon as it is done, so the parent watches them appear and a failure redoes one page, not the book.
  The job runner is chosen in the first task: Cloudflare Workflows (Python is supported, one step per picture: developers.cloudflare.com/workflows/python/) or a job table in Postgres worked by the container.
- Every picture prompt goes through the safety check (Slice 5).

**Work**
- [ ] Cast agent: reads the story and lists characters and recurring places, each with a look description the parent can edit (a "Cast" screen).
- [ ] Style choice: 3-4 house styles with a sample each, starting from ReadTheStupidText `prompts.md`.
- [ ] Reference sheets per character and place, shown for approval; "Redraw" per sheet.
- [ ] Page pictures with the approved sheets as reference images; progress screen with each page appearing; "Redraw this page" with an optional note.
- [ ] Picture eval: the Slice 1 vision-judge prompt, plus an embedding check: an image embedding of each character crop compared with its reference sheet, flagging pages below a threshold.
      Measure how well the embedding score agrees with the judge and with the owner's scores on 3 test books.
- [ ] Pick the judge model and the illustrator's prompt-writing model from the evals (`evals/README.md`).
- [ ] The PDF from Slice 7 now uses the real pictures.

**Verification**
- A 12-page test book gets 12 pictures; killing the job halfway and resuming redraws only the missing pages.
- Picture eval table with judge/owner agreement in `evals/README.md`.
