### Slice 1 — Spike: Gemini vs OpenAI pictures, quality and cost

*Planned.*

**Touches:** `spikes/pictures/` (new), `docs/plan/notes/` (new)

**Why.** Today every picture is made by hand in the free Gemini website, inside the owner's signed-in Chrome
(ReadTheStupidText `docs/plan/decisions.md` Decision 125). A public app has to call a paid picture service
directly. The owner asked for a spike that compares Gemini and OpenAI on quality and cost before we pick one
(owner, 2026-11-26: "lets have a spike and compare both their quality and cost").

**Decisions locked with the owner**
- Compare both services on the same book. Nothing else is built on top until the spike picks a winner.
- Free starting credits must cover "a small book with low quality images", so the spike also prices the cheapest usable quality.

**Work**
- [ ] A small Python script (`spikes/pictures/`, using `uv`) that takes one short test story (8 pages, 2 characters, 1 recurring place),
      makes a reference sheet for each character and the place, then draws every page with those sheets passed as reference images.
      Start from ReadTheStupidText `.claude/agents/dino-story-illustrator.md` and its `prompts.md` (house style, 896x1200 portrait).
- [ ] Run it against the current Gemini image model and the current OpenAI image model, at their lowest and highest quality setting.
      Model IDs and prices are read from the vendors' docs on the day (`library-docs`), not from memory.
- [ ] Record per run: cost per picture and per book (from the API usage fields, not the price page alone), time per picture,
      largest size, and whether several reference images fit in one request.
- [ ] Score consistency: a Claude vision judge rates each page against the reference sheets (same face, colours, clothes, 1-5),
      and the owner scores the same contact sheet by eye. Keep both scores; the judge prompt becomes the first picture eval (Slice 8).
- [ ] Check print size: can each service give at least 2588x2625 px (KDP 8.5in square at 300 dpi with bleed), or does it need upscaling?
- [ ] Write `docs/plan/notes/2026-11-xx-picture-service-spike.md`: contact sheets, the numbers table, the recommendation, and the price of a
      12-page low-quality book (the free starting-credit size).

**Verification**
- Both contact sheets exist in the note and the table has a real cost for every run.
- The owner picks the service; the pick is written into this file and into Slice 8 the same turn.
