### Slice 10 — Text effects: a colour per speaker and playful words

*Planned.* **Starts after:** Slice 7.

**Touches:** `api/src/studio/agents/styler/` (new), `web/src/features/text-style/` (new), `pdf/src/Styling/`, `evals/datasets/styling/` (new)

**Why.** "edits on text like the color per voice, playful things, etc. that I used for my amazon books" (owner, 2026-11-26).
The rules already exist in print-the-damn-book `docs/design/kdp-typography.md` and `src/PrintTheDamnBook.Core/Kdp/Styling/`:
speaker colours (R5), big and small words (R2), colour words (R3), sound triplets like splish, splash, splosh (R4), words that move down, up or loud (R6),
refrains (R7), shouts in bold (R8), stretched words (R9) and sung lines with a bounce (R10).

**Work**
- [ ] Styler agent: marks who says each quoted line, and which words get which effect, as structured output over the page text (character offsets, never rewriting text).
      Starts from the print-the-damn-book `kdp-style` skill.
- [ ] Speaker colours chosen from the cast with readable contrast on the page background; the parent can change any colour.
- [ ] Text style screen: the page with effects shown; tap a word to change or remove its effect; a switch to turn all effects off.
- [ ] The PDF service draws the effects (port the existing `Styling/` code in `pdf/`).
- [ ] Styling eval: 40 labelled pages (speaker of each quote, effect words). Measure speaker accuracy and effect precision; pick the model (`evals/README.md`).

**Verification**
- Speaker accuracy and effect precision in `evals/README.md`; golden PDF test with every rule R2-R10 on one page.
