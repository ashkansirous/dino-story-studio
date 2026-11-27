### Slice 17 — Preview on the web, the free preview PDF, and the print-quality PDF for credits

*Planned.* **Starts after:** Slices 7, 8, 9 and 11.

**Touches:** `web/src/features/preview/` (new), `api/src/studio/routes/books.py`, `api/src/studio/routes/print_pdf.py` (new), `api/src/studio/jobs/`, `pdf/` (read; render calls only)

**Why.** Owner brief (2026-11-27): "Preview & download has a free preview PDF and a print-quality PDF for credits. It has no rebuild button." The plan had a plain "Download PDF" button in Slice 7 and no preview screen.

**Decisions**
- The preview shows the whole book on the web in spreads, built from the current story, pictures, covers and extra pages. There is no rebuild button: the preview and the free PDF are built from the current state each time the screen opens (a loading state, with Cancel, while it builds), so they are never stale.
- Free: the preview PDF, made from the draft pictures ("Fine for reading on a screen or a quick print at home").
- Paid: the print-quality PDF. "We redraw every page's current picture in full quality (the same picture, 300 dpi), then build the PDF with bleed and the cover PDF (back, spine, front), ready for a print service." The redraw job is Slice 8's; the PDF is Slice 7's service; the price and the debit are Slice 11's. It is the only place pictures are drawn at full quality.
- Once paid, "Download again any time from My books." The paid files are kept; downloading again costs nothing. A later change to the book does not change the paid files; making new ones is a new purchase.

#### Design (App Design mock, section 13 "Preview on the web, free download, print PDF for credits" (board 13; devices 26, 39, 52, 65), added 2026-11-27)

One board with two states ("Putting your book together…" and the finished preview), at five sizes. Simplified illustrations: the page boxes are coloured rectangles standing in for the real rendered pages.

- Book header with the step bar: "✓ Story" through "✓ Extra pages", "9 Preview & download" current.
- **Building state:** h1 "Putting your book together…", "Page 13 of 24", link "Cancel" (to Extra pages).
- **Ready state:** h1 "Your book"; the spread viewer on a dark canvas: label "Pages 1 of 24" for the front cover alone, then "Pages 2–3 of 24", "Pages 4–5 of 24" and so on, previous and next icon buttons (aria-labels "Previous pages", "Next pages", 44px), the pages drawn square (max 380px alone, 320px each in a spread, never wider than 48% of the row), cover with the title in Fredoka 700 26px #5A2E0C, text pages in Andika 18px/1.4 centred, picture pages tinted, blank pages white, "The End" in Fredoka 700 34px #8A4B12. Below, a strip of spread buttons ("1", "2–3", "4–5" …, min 52×44px, current = 2px `--primary` on `--primary-tint`; aria-label "Pages 2 and 3"). The order of pages is: front cover, extra pages that are on (This book belongs to, dedication, credits), the story as text page then picture page for each story page, blank pages up to the count, "The End" last (the mock draws the facing layout, so the text page and picture page alternate; with "Words on the picture page" the text sits on the picture).
  Two cards on the side. "Preview PDF": "Uses your draft pictures. Fine for reading on a screen or a quick print at home." and a button "Free download". "Print-quality PDF": the paragraph quoted in Decisions, then the button "Make print-quality PDF · [CREDITS] credits" and "You have 120 credits." After purchase it becomes two buttons, "Download print PDF" and "Download cover PDF", and "Paid for. Download again any time from My books."
- Phone landscape: the viewer and the cards sit side by side, each scrolling on its own.

**Work**
- [ ] Preview screen: spread viewer, thumbnails, the building state with Cancel, loading states for the page images.
- [ ] Free preview PDF: render through the Slice 7 service with the draft pictures and download it.
- [ ] Print-quality purchase: check the balance (Buy credits pop-up if short), debit once, start the full-quality redraw job (Slice 8) for the current picture of every page, then the PDF with bleed and the cover PDF (Slice 9's cover sizes); progress per picture; a failed picture is retried and a partly done job resumes without charging again.
- [ ] After purchase: store the files, show the two download buttons, and show the same buttons from the book's card in My books (the card opens the book at Preview).
- [ ] Playwright: open Preview, page through the spreads, download the free PDF (right page count and size), buy with enough credits (fake Stripe and fake picture service), and download both paid files; with 1 credit the pop-up opens and nothing is debited.

**Verification**
- The free PDF page count equals the preview's page count, at both sizes.
- The purchase debits once even if the screen is reloaded halfway; downloads after purchase are free.

**Plan changed to match the mock (2026-11-27):**
- A plain "Download PDF" on the format screen (Slice 7) becomes a preview screen with a free preview PDF and a paid print-quality PDF, with no rebuild button.
- Full-quality pictures are made only at purchase (Slice 8).
