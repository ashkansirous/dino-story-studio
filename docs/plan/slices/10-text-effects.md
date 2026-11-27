### Slice 10 — Text effects: a colour per speaker and playful words

*Planned.* **Starts after:** Slices 7, 14 and 18.

**Touches:** `api/src/studio/agents/styler/` (new), `web/src/features/text-style/` (new), `pdf/src/Styling/`, `evals/datasets/styling/` (new)

**Why.** "edits on text like the color per voice, playful things, etc. that I used for my amazon books" (owner, 2026-11-26).
The rules already exist in print-the-damn-book `docs/design/kdp-typography.md` and `src/PrintTheDamnBook.Core/Kdp/Styling/`:
speaker colours (R5), big and small words (R2), colour words (R3), sound triplets like splish, splash, splosh (R4), words that move down, up or loud (R6),
refrains (R7), shouts in bold (R8), stretched words (R9) and sung lines with a bounce (R10).

#### Design (App Design mock, section 9 "Text effects (R1–R10 from print-the-damn-book)" (board 9; devices 22, 35, 48, 61), added 2026-11-27)

One desktop board with its four device sizes. The mock agrees with the plan's rules and adds the screen: a sample page with every effect, one switch per effect, editable speaker colours. The sample page text is a simplified illustration (a fixed demo paragraph), not the parent's page; the real screen shows the parent's pages.

- Book header: "✓ Story" to "✓ Pictures", "5 Text effects" current. Layout "cols": settings in a side panel (`<aside aria-label="Effect settings">`), the sample page in the main area.
- h1 "Text effects"; "The styler marks each word for you. Turn any effect on or off for the whole book, or tap a word to change just that one."
- Preview card: the label "Sample page · every effect" and the count "16 of 16 on"; seven sample paragraphs showing each effect with the page's own font (Andika, 18px-25px as the effect asks): speaker colours on quotes, "Red Hen" in red bold, "tiny" small, "softly" and "quietly" small grey italic, "great big" large bold, triplets "woof, woof, woof" growing, "down, down, down" stepping down, "splish, splash, splosh" bouncing in three colours (#C2410C, #1F5FA8, #7A3E9D), "rumble" tilting, "up" stepping up, "tap, tap, tap" shrinking, "long" and "slow" spaced out, a sung line "La la la, warm bread for all!" bouncing in the singer's colour.
- Side panel: "All effects" master switch (48×28px; caption "Off gives plain text in one colour"; aria-label "All effects"; on while any effect is on). Then five groups, each a small-caps title and one switch row per rule (label and a one-line hint):
  VOICES: "Speaker colours" "Quoted words in the speaker’s colour".
  WORD SIZE: "Big words" "big, huge, giant… bigger and bold"; "Small words" "little, tiny… smaller"; "Quiet words" "softly, gently, whisper… small, italic, grey".
  COLOUR AND SOUNDS: "Colour words" "red hen, blue boat… in that colour"; "Sound triplets" "splish, splash, splosh… three colours, bold".
  WORDS THAT MOVE (ON TRIPLETS): "Going down" "down, fall, drop… steps down"; "Going up" "up, high, climb… steps up"; "Loud sounds" "calls and animal noises… grow"; "Quiet sounds" "a triplet said quietly… shrinks"; "Shaking" "rumble, wobble, shiver… tilts"; "Bouncing" "splash, hop, jump… bounces".
  LINES: "Refrains" "a line said 2+ times in the book, big and bold"; "Shouts" "1–3 words ending in ! at the start of a quote"; "Stretched words" "long, slow, sleepy… spaced out"; "Songs" "sung lines bounce in the singer’s colour".
- h2 "Speaker colours": "Picked for you from each character's sheet. Tap a swatch to change it: every choice is dark enough to read, never red, and different from the others." One row per speaker (the sample: Little Red Hen #8A4B12, The Cat #0E6B78, The Dog #2F6B34): a 44px swatch (radius 12px; open: 3px `--text` border) and the name; tapping opens a palette (aria-label "Colour for The Cat") of ten 44px swatches (#1F4FA0, #0E6B78, #6A3A96, #2F6B34, #8A4B12, #5B3A8C, #1D5C5C, #7A4A2A, #3A4F8A, #4A6B1F). A colour already used by another speaker is faded (opacity .25, not selectable, aria-label ends "(used)"). Note: "Faded colours are already used by another speaker."
- Link "Next: front cover →".
- Phone landscape: preview and settings side by side, each scrolls alone. New strings: all quoted lines above.

**Work**
- [ ] Styler agent: marks who says each quoted line, and which words get which effect, as structured output over the page text (character offsets, never rewriting text).
      Starts from the print-the-damn-book `kdp-style` skill.
- [ ] Sixteen effect switches as drawn (the plan's R2-R10 split as the mock shows: R2 into big, small and quiet words; R6 into going down, going up, loud sounds, quiet sounds, shaking and bouncing), a master "All effects" switch, and a stored on/off per effect per book.
- [ ] Speaker colours picked from the cast sheets (Slice 18) from the ten-colour palette with readable contrast on the page background, never red, no two speakers the same; the parent can change any colour.
- [ ] Text style screen: the sample page with effects shown, the settings, and tap a word on the parent's real page to change or remove its effect (the intro line promises it; the mock does not draw the word menu, so it is a small popover with the effect list and "Remove effect").
- [ ] The PDF service draws the effects (port the existing `Styling/` code in `pdf/`), honouring each switch.
- [ ] Styling eval: 40 labelled pages (speaker of each quote, effect words). Measure speaker accuracy and effect precision; pick the model (`evals/README.md`).

**Verification**
- Speaker accuracy and effect precision in `evals/README.md`; golden PDF test with every rule R2-R10 on one page, and one with every switch off.
- Playwright: turn one effect off, change a speaker colour (a used colour cannot be picked), and the preview and the PDF follow.
- Screenshots at the five sizes are in the PR.

**Plan changed to match the mock (2026-11-27):**
- The plan's "switch to turn all effects off" is the "All effects" master switch; the plan's rules R2-R10 became sixteen individually switchable effects (the mock draws them; the plan listed nine rules).
- "Quiet words" (R2 variant) is added as drawn.
- Speaker colours: the plan said "readable contrast"; the mock adds "never red" and "different from the others" and a fixed ten-colour palette.
- Order: Text effects follows Pictures and precedes Front cover.
