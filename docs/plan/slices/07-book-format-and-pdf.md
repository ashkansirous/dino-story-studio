### Slice 7 — Book size, text placement and the PDF service (text-only first)

*Planned.* **Starts after:** Slice 3.

**Touches:** `pdf/` (new), `web/src/features/format/` (new), `api/src/studio/routes/books.py`, `api/src/studio/pdf_client.py` (new)

**Why.** "then they choose the size that they want to generate a book in. for example, if they want to print at home, they'll have the a5 on each page.
they can decide to allow 30% on buttom of their pages for the text or have the text beside the page" (owner, 2026-11-26).
Decision 3 (owner, 2026-11-26): reuse the C# book builder from print-the-damn-book as a separate service rather than rewriting it.

**Decisions**
- Sizes in the first version: A5 portrait for home printing (no bleed, printer margins) and the KDP 8.5in square with 0.125in bleed (print-the-damn-book `src/PrintTheDamnBook.Core/Kdp/KdpTrim.cs:6-27`).
- Text placement: "band" (text in the bottom 30% of the picture page, on a soft tint) or "facing" (text on its own page opposite the picture, as the owner's KDP books do).
- The PDF service is C# (.NET 10, QuestPDF), everything else is Python. Recorded as decided by this planning session from Decision 3; the owner said "this needs to be implemented with python" for the app and AI work.
  If the owner wants the PDF part in Python too, that is a new slice, not a change here.
- The service takes a JSON book (pages, text runs with styles, picture URLs, size, placement) and returns a PDF; it does not read `.dino` files.

**Work**
- [ ] Copy (not link) the needed code from print-the-damn-book `src/PrintTheDamnBook.Core/Kdp/` and `src/PrintTheDamnBook.Kdp/` into `pdf/`, with the Andika and Vazirmatn fonts and their licences.
      Note the source commit in `pdf/README.md`.
- [ ] Small HTTP API: `POST /render` (JSON book in, PDF out) and `GET /health`; a Dockerfile for Linux with SkiaSharp native assets.
- [ ] Add the A5 trim and the "band" placement; keep the square trim and "facing" placement behaving as print-the-damn-book does today.
- [ ] Page count rules: KDP needs at least 24 pages and an even count (`Core/Kdp/KdpPagePlan.cs:43,88-93`); fill with extra pages from Slice 9 or blank pages, and tell the parent.
- [ ] Format screen in the web app: size and placement as picture choices with a live preview of one page (grey box where the picture goes).
- [ ] "Download PDF" works with grey placeholder pictures until Slice 8 is built. Rendering over 500 ms shows progress.

**Verification**
- Golden-file tests in `pdf/` for both sizes and both placements (page size, bleed, text box position).
- Playwright: choose A5 + band, download, and the PDF has the story's page count at A5 size.
