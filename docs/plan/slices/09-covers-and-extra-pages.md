### Slice 9 — Front and back cover, and extra pages inside the book

*Planned.* **Starts after:** Slice 8.

**Touches:** `api/src/studio/agents/cover/` (new), `web/src/features/covers/` (new), `pdf/src/Matter/`

**Why.** "after that, they'll have options for back and front cover, and for internal extra pages" (owner, 2026-11-26).
The owner's books already have these: print-the-damn-book `src/PrintTheDamnBook.Core/Kdp/KdpMatterSpec.cs` and `docs/design/kdp-typography.md`
(credits, "this book belongs to", dedication), plus a back-cover picture per book.

**Work**
- [ ] Front cover: title, "by <name>", a cover picture made from the cast sheets (2-3 choices to pick from), title placement choices.
- [ ] Back cover: a short blurb written by AI from the story (editable), a back picture or a colour from the book's palette.
- [ ] Extra pages, each switchable: "This book belongs to", dedication (text box), credits ("Story by", "Pictures made with AI"), "The end" page.
      Leave out the owner's own "More books" and Dino QR pages; they are for the owner's store books.
- [ ] For the KDP size, a full cover PDF (back + spine + front) sized from the page count; for A5, separate front and back pages.
- [ ] Extra pages also fill the KDP 24-page minimum before blank pages are used (Slice 7).

**Verification**
- Golden-file tests for the cover sizes; Playwright: turn every extra page on, download, and each appears in order.
