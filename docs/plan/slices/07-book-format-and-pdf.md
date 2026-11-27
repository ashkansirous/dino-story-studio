### Slice 7 — Book size, text placement and the PDF service (text-only first)

*Planned.* **Starts after:** Slices 3 and 14.

**Touches:** `pdf/` (new), `web/src/features/layout/` (new), `api/src/studio/routes/books.py`, `api/src/studio/pdf_client.py` (new)

**Why.** "then they choose the size that they want to generate a book in. for example, if they want to print at home, they'll have the a5 on each page.
they can decide to allow 30% on buttom of their pages for the text or have the text beside the page" (owner, 2026-11-26).
Decision 3 (owner, 2026-11-26): reuse the C# book builder from print-the-damn-book as a separate service rather than rewriting it.
Owner brief (2026-11-27): "Size & layout comes before pictures. The 'text on the page (30% band)' option exists only for portrait pages, and only when the text fits."

**Decisions**
- Sizes in the first version: A5 portrait for home printing (no bleed, printer margins) and the KDP 8.5in square with 0.125in bleed (print-the-damn-book `src/PrintTheDamnBook.Core/Kdp/KdpTrim.cs:6-27`).
- Text placement: "same page" (text in the bottom 30% of the picture page, kept free of art) or "own page" (text on its own page opposite the picture, as the owner's KDP books do). ~~Both placements for both sizes.~~ Superseded 2026-11-27 (the mock supersedes this): "same page" exists only for the A5 portrait size and is offered only when every page's text fits the band; the square size always puts the words on their own page.
- "Fits" is measured by the PDF service on the real text and font (`POST /measure`), not counted in words; the screen shows the longest page's word count only as the explanation line.
- The step is third in the book (after Story and Cast) and before Pictures: the illustrator draws to this shape and keeps the bottom 30% calm when the words sit on the picture (Slice 8 reads the choice).
- The PDF service is C# (.NET 10, QuestPDF), everything else is Python. Recorded as decided by this planning session from Decision 3; the owner said "this needs to be implemented with python" for the app and AI work.
  If the owner wants the PDF part in Python too, that is a new slice, not a change here.
- The service takes a JSON book (pages, text runs with styles, picture URLs, size, placement) and returns a PDF; it does not read `.dino` files.
- The download buttons are not on this screen: the free preview PDF and the paid print PDF are on the Preview screen (Slice 17).

#### Design (App Design mock, section 7 "Book size and where the words go (before pictures)" (board 7; devices 20, 33, 46, 59), added 2026-11-27)

One desktop board with two states (text fits / does not fit) and its four device sizes. The mock changes the plan's order (this step is before pictures), which options are allowed, and drops the download button from this screen. The page preview shows coloured shapes for the picture: a simplified illustration.

- Book header: "✓ Story", "✓ Cast", "3 Size & layout" current (Slice 14). Layout "cols": the settings in the left panel (flex 1 1 380px, gap 22px), the preview on the right (flex 999 1 520px).
- Left panel: h1 "Book size and layout" (Fredoka 600 32px); "Decide this before the pictures: the illustrator draws to this shape and keeps space for the words."
- h2 "Book size": radio group "Book size", two cards in two columns (radius 14px, padding 14px): an outline icon 30×42px "A5 portrait" "Print at home · no bleed"; an outline icon 40×40px "8.5in square" "Print book · 0.125in bleed".
- h2 "Where the words go": radio group "Text placement": "Words on the picture page" "Bottom 30% kept free for the words" and "Words on their own page" "Words opposite the picture, the picture uses the whole page". A disabled option has a dashed #E3DACB border, #F7F3EC fill, opacity .6, `aria-disabled`, not-allowed cursor. Below, one explanation line (radius 12px, 14px): square: "Square pages always have the words on their own page."; A5 and fits: "Every page is short enough to fit in the bottom 30% (longest: 24 words)." on `--soft`; A5 and does not fit: "Page 3 has 36 words — too many for the bottom 30%. Shorten it, or keep the words on their own page." on `--warn-bg` with the link "Shorten page 3 in the editor →" (to Story).
- A green notice card: "THE ILLUSTRATOR WILL DRAW" (12px, 800, #1B5E20) and the brief: "Portrait pages, A5" or "Square pages, 8.5in with bleed", then ", using the whole picture." (own page) or ", with the bottom 30% kept calm and plain for the words." (same page).
- Button "Next: draw the pages →" (`--primary`).
- Preview (right): the line "Preview · page 2" and the size name ("A5 · 148 × 210 mm" or "8.5 × 8.5 in + bleed"), on a #EFE7DA canvas (radius 20px, padding 36px): the page at A5 aspect 1:1.414 (250px wide on its own page, 330px with the words on it) or square (290px / 380px), with the shadow 0 8px 24px rgba(60,40,10,.18). Own page: a picture page and a text page side by side with the page text ("Who will help me plant it?" asked Hen. "Not I," said the Cat. "Not I," said the Dog.); same page: the picture with the label "KEPT FREE FOR TEXT · 30%" over the bottom 30% holding the text.
- Defaults: A5 portrait, "Words on the picture page" when allowed, else "Words on their own page"; choosing the square size switches to own page.
- Phone landscape: panels side by side, each scrolls on its own. New strings: all quoted lines above.

**Work**
- [ ] Copy (not link) the needed code from print-the-damn-book `src/PrintTheDamnBook.Core/Kdp/` and `src/PrintTheDamnBook.Kdp/` into `pdf/`, with the Andika and Vazirmatn fonts and their licences.
      Note the source commit in `pdf/README.md`.
- [ ] Small HTTP API: `POST /render` (JSON book in, PDF out), `POST /measure` (which pages' text fits the 30% band) and `GET /health`; a Dockerfile for Linux with SkiaSharp native assets.
- [ ] Add the A5 trim and the "same page" 30% band placement; keep the square trim and the "own page" placement behaving as print-the-damn-book does today.
- [ ] Page count rules: KDP needs at least 24 pages and an even count (`Core/Kdp/KdpPagePlan.cs:43,88-93`); fill with extra pages from Slice 9 or blank pages, and tell the parent (the A5 size has no minimum drawn in the mock; it follows the plan's KDP-only rule).
- [ ] Size & layout screen as drawn: size cards, placement cards with the disabled state and the explanation line, the illustrator brief, the live page preview. The choice is saved on the book and read by Slices 8, 9, 10 and 17.
- [ ] Rendering over 500 ms shows progress. A placeholder-picture render (grey boxes) is kept for tests until Slice 8 is built; the download button is Slice 17's.
- [ ] If the parent changes size or placement after pictures exist, the pictures stay and the pages are marked "drawn for another shape" so the parent can choose to redraw (Slice 8); nothing is redrawn by itself.

**Verification**
- Golden-file tests in `pdf/` for both sizes and both placements that are allowed (A5 same page, A5 own page, square own page): page size, bleed, text box position; `/measure` says page 3 of the sample (36 words) does not fit and the others do.
- Playwright: choose A5 + same page, then square (placement switches to own page and the same-page card is disabled), and the saved choice survives a reload.
- Screenshots at the five sizes are in the PR.

**Plan changed to match the mock (2026-11-27):**
- Order: this step comes before Pictures (Size & layout is step 3 of 9); the plan had "Format screen" after Slice 3 with pictures later, and said the PDF "works with grey placeholder pictures until Slice 8" (the PDF download moves to Slice 17).
- Placement options: the square size always uses "own page"; "same page" only for A5 and only when the text fits (the plan allowed both placements for both sizes; the mock supersedes this).
- Names: "band" / "facing" become the screen's "Words on the picture page" / "Words on their own page".
- Added: the illustrator brief card, the explanation line with the link back to the editor, the page preview with the labelled 30% band.
