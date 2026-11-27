### Slice 9 — Front and back cover, and extra pages inside the book

*Planned.* **Starts after:** Slices 8 and 14.

**Touches:** `api/src/studio/agents/cover/` (new), `web/src/features/covers/` (new), `pdf/src/Matter/`

**Why.** "after that, they'll have options for back and front cover, and for internal extra pages" (owner, 2026-11-26).
The owner's books already have these: print-the-damn-book `src/PrintTheDamnBook.Core/Kdp/KdpMatterSpec.cs` and `docs/design/kdp-typography.md`
(credits, "this book belongs to", dedication), plus a back-cover picture per book.

**Decisions**
- Covers and extra pages are never drawn automatically (owner brief, 2026-11-27: "Show an editable suggested prompt and wait for Draw"). Words and where they sit are free; only drawing a picture uses credits, and only when the parent presses Draw.
- The title, the author name and the blurb are text boxes the parent drags on a large preview (and moves with the arrow keys, 1% a step); they stay inside a dashed safe line so print does not cut them. The positions are saved with the book and used by the PDF service. This replaces the plan's "title placement choices" (the mock supersedes this).
- Each Draw makes one picture, and keeps the earlier ones as "tries" the parent can pick again for free. This replaces the plan's "2-3 choices to pick from" made at once (the mock supersedes this).
- Cover pictures are drawn once at print quality, at one price per Draw (the mock shows one price on the button and no second step); the cover PDF therefore needs no redraw at purchase (Slice 17).
- The author name on the cover comes from the account's name (Slice 15) and can be changed on each book.
- Every cover prompt goes through the safety check (Slice 5).

#### Design (App Design mock, sections 10, 11 and 12 (boards 10-12; devices 23-25, 36-38, 49-51, 62-64), added 2026-11-27)

Three desktop boards at five sizes each. The mock agrees with the plan on content (front, back, extra pages) and changes how a cover is made (one Draw at a time from an editable prompt; boxes dragged on the preview). The cover pictures in the boards are coloured shapes: simplified illustrations.

- Common: step bar with the earlier steps ✓; "6 Front cover", "7 Back cover" or "8 Extra pages" current; layout "cols" with the settings on the left and a large preview on the right (the cover box is 600px wide max, square in the mock, `container-type: inline-size`, radius 4px, shadow 0 8px 24px rgba(60,40,10,.18); the real cover follows the chosen size's trim).
- **Front cover** (`app/CoverFront.dc.html`): above the preview "Drag the title and your name anywhere. Keep them inside the dashed line so print doesn't cut them." The preview before drawing: an empty #FFFCF9 box with "No picture yet" and "Check the prompt, then press Draw". Two draggable boxes with a dashed border (rgba(90,46,12,.45), radius 8px, `touch-action: none`, grab cursor; focused or dragged: 3px `--primary` outline): the title in Fredoka 700 up to 40px (`clamp(18px, 6.6cqw, 40px)`, colour #5A2E0C) at x 12% y 11%, and "by Ash" in Nunito 800 up to 20px at x 38% y 36%; aria-labels "Title box. Drag, or use arrow keys to move" and "Author name box. Drag, or use arrow keys to move". After the first Draw, a strip "Your tries · tap one to use it again (free)" with 72px square thumbnails ("Use try 2", selected 3px `--primary`).
  Left panel: h1 "Front cover"; "Words and where they sit are free to change. Only drawing the picture uses credits."; fields "Title" (the book title) and "By"; "Picture prompt · suggested from your story, edit anything" (text area: "Little Red Hen standing proudly in the farmyard, holding a warm golden loaf, morning light, the barn behind her. Keep space calm where the title and name sit."); "Uses your sheets:" chips ("Little Red Hen", "The farmyard", "Soft watercolour"); CostButton "Draw the cover · [CREDITS]" (after the first: "Redraw with this prompt · [CREDITS]"); "You have 120 credits"; link "Next: back cover →" (soft grey until a picture exists, `--primary` after; it is a link at all times).
- **Back cover** (`app/CoverBack.dc.html`): above the preview "Drag the blurb anywhere. Keep it inside the dashed line so print doesn't cut it." The blurb box (dashed border rgba(43,37,64,.4), radius 12px, white 90% fill, width 60%, padding 14px 16px, starts at x 12% y 12%, may move up to 40% across and 80% down; aria-label "Blurb box. Drag, or use arrow keys to move").
  Left panel: h1 "Back cover"; "The blurb and a colour background are free. A picture uses credits, and is only drawn when you press Draw."; "Blurb · written from your story, edit anything" (text area, sample: "Nobody wants to help Little Red Hen plant, water or bake. So what will she do when the warm bread comes out of the oven?") and the button "Suggest another blurb · free"; "Background" radio group, two cards: "A colour from the book" "Free" and "A picture" "Drawn when you press Draw"; for the colour: swatches 44px, radius 12px, group "Back cover background colour": Apricot #F6C99A, Meadow green #9ED8A4, Sky blue #B5CDE4, Wheat #F2E2B6; for the picture: "Picture prompt · suggested, edit anything" (sample: "Little Red Hen from behind, walking home across the farmyard at sunset with the empty bread basket. Keep space calm where the blurb sits.") and CostButton "Draw the back picture · [CREDITS]" (after: "Redraw with this prompt · [CREDITS]"); "You have 120 credits"; link "Next: extra pages & PDF →".
- **Extra pages** (`app/Covers.dc.html`): h1 "Extra pages"; "These pages have no pictures, so they're free. Switch on the ones you want."; h2 "Extra pages" and four rows with a switch (48×28px; on = `--primary`, off #C9C1B3): "This book belongs to" "A line for the child's name"; "Dedication" with a text field (aria-label "Dedication text", sample "For [NAME], who always shares"); "Credits" "Story by · Pictures made with AI"; "The end". h2 "Your book so far": a list "Size" "8.5in square + bleed", "Story pages" "12", "Extra pages" "3", "Blank pages added" "9", "Total" "24 pages"; "Print books need at least 24 pages. Turn on "The end" to use one fewer blank page."; "You also get a full cover PDF: back, spine and front, from your front and back covers." (links to the two cover screens); link "Next: preview your book →".
- Phone landscape: settings and preview side by side, each scrolls on its own; the preview box shrinks with its pane.
- New strings: all quoted lines above.

**Work**
- [ ] Front cover screen: title, "by <name>", the editable suggested prompt (written from the story and the cast sheets), Draw and Redraw as CostButtons, tries strip, draggable title and name boxes with keyboard movement; positions saved.
- [ ] Back cover screen: a blurb written by AI from the story (editable; "Suggest another blurb" is free), the choice of a palette colour or a picture, the picture prompt and Draw, the draggable blurb.
- [ ] Extra pages, each switchable: "This book belongs to", dedication (text box; "[NAME]" in the sample is filled from the reader's name given in Slice 16, else left for the parent to edit), credits ("Story by", "Pictures made with AI"), "The end" page.
      Leave out the owner's own "More books" and Dino QR pages; they are for the owner's store books.
- [ ] The "Your book so far" summary: size, story pages, extra pages on, blank pages added, total; computed with Slice 7's page-count rules.
- [ ] For the KDP size, a full cover PDF (back + spine + front) sized from the page count; for A5, separate front and back pages. Both are produced at purchase (Slice 17).
- [ ] Extra pages also fill the KDP 24-page minimum before blank pages are used (Slice 7).

**Verification**
- Golden-file tests for the cover sizes; Playwright: drag the title (and use the arrow keys), draw a cover with the fake picture service, turn every extra page on, and each appears in order in the Preview (Slice 17).
- Nothing is drawn until Draw is pressed; with too few credits the Buy credits pop-up opens and nothing is charged.
- Screenshots at the five sizes are in the PR.

**Plan changed to match the mock (2026-11-27):**
- "2-3 cover choices to pick from" becomes one Draw at a time with free re-use of earlier tries; "title placement choices" becomes draggable boxes (the mock supersedes both).
- Never drawn automatically; an editable suggested prompt waits for Draw (owner brief).
- Back cover: a colour from the palette (free) or a drawn picture (credits), as the plan's "a back picture or a colour from the book's palette".
- Extra pages: the "Your book so far" summary and the cover-PDF note are added.
