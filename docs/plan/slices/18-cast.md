### Slice 18 — Cast: pick a picture style, check everyone in words, draw each one at a time

*Planned.* **Starts after:** Slices 1, 3 and 14. (The cast agent and the reference sheets moved here from Slice 8; credits are charged through Slice 11 once it exists, with the price shown from the start.)

**Touches:** `api/src/studio/agents/cast/` (new), `api/src/studio/agents/sheet_artist/` (new), `web/src/features/cast/` (new), `evals/datasets/cast/` (new), `evals/cast/` (new)

**Why.** Owner flow (2026-11-26): the illustrator keeps characters the same with one reference sheet per character and a sheet per recurring place (ReadTheStupidText `.claude/skills/create-dino-story/SKILL.md`, "Who does what"). The owner's brief of 2026-11-27 puts this in the app as step 2: "Cast (style samples → descriptions → each character drawn one at a time)".

**Decisions**
- Order inside the step: 1 Style, 2 Describe the cast, 3 Draw each one. Style samples are ready-made, so choosing is free. Words are free. Each sheet costs `[CREDITS PER SHEET]` and the parent approves each before the next is drawn.
- Nothing is locked: the parent can come back at any time and look again, change the words or redraw, even after approving. Changing someone who is already drawn marks them for a redraw; pages already drawn with the old sheet are offered a redraw (never redrawn by themselves, see Slice 8).
- The cast agent reads the story and lists characters and recurring places, each with a look description (this is the plan's Slice 8 item, unchanged). The style list is "Soft watercolour", "Bold flat shapes", "Crayon and pencil", "Cosy 3D clay" (the plan said 3-4 house styles; the mock draws four).
- Every sheet prompt goes through the safety check (Slice 5). Real names never reach the model (Slice 16).

#### Design (App Design mock, section 6 "Cast: style, describe, draw one by one" (board 6; devices 19, 32, 45, 58), added 2026-11-27)

One desktop board with three internal sub-steps and four device sizes. The style swatches and the character sheet figure are simplified illustrations (coloured shapes standing in for real art), not UI to build; the real art is the sample images and the drawn sheets. The mock disagrees with the plan on the order (Cast comes before Size & layout and Pictures) and on drawing (one at a time, each approved); changed below.

- Book header (Story ✓, Cast current) and a sub-step bar `<ol aria-label="Cast steps">` of three pills (min-height 44px, radius 999px, Nunito 800 15px): "1 Style", "2 Describe the cast", "3 Draw each one". Current: 2px `--primary` border on `--primary-tint`, #1B5E20 text; done: white with `--primary` text; later: white with `--muted` text. Each is clickable at any time.
- **1 Style:** h1 "Pick a picture style"; "These are ready-made samples, so choosing is free. Nothing of yours is drawn yet."; a radio group "Picture style" of four cards (radius 16px, padding 10px; selected: 3px `--primary` border), each a 150px-high sample swatch and its name; button "Next: check the cast".
- **2 Describe the cast:** h1 "Check how everyone looks — in words"; "We found these in your story. Fixing words is free; redrawing a picture costs credits, so get the details right here first. You can come back any time: if you change someone who's already drawn, they're marked for a redraw."; per character a card with the name, a kind tag ("Main character", "Character", "Place") and a text area "How <name> looks" (sample: "Small russet-red hen, yellow beak, a blue checked apron with a pocket."); the cast in the mock is Little Red Hen, The Cat, The Dog, The Duck and The farmyard; button "Draw Little Red Hen · [CREDITS]" (the first one to draw) with "Each sheet costs [CREDITS PER SHEET]. You approve each one before the next is drawn."
- **3 Draw each one:** a side list `<aside aria-label="Cast progress">`: one row per cast member (min-height 48px, radius 12px; open = 2px `--primary` border) with a status tag "Open" (warn colours), "Approved" (`--primary-tint`, #1B5E20) or "Waiting" (`--soft`, `--muted`), and the line "Tap anyone to look again, change their words or redraw them — even after approving."
  Main area: a counter "1 of 5", h1 the name, the sheet picture (260×300px, radius 14px, a 2×2 grid of figures on #FBF3E8 in the mock; the real sheet is the drawn picture), label "How they look" with a text area, the note "Approved already. If you redraw, any pages already drawn with the old sheet will be offered a redraw too." when approved, label "Not quite right? Tell the illustrator" with the field "e.g. make the apron blue, smaller beak", and two buttons: "Approve and draw The Cat · [CREDITS]" (the label names the next unapproved character; "Approve" when none is left) and "Redraw · [CREDITS]".
  When all are approved: h1 "Everyone is ready", "Every page will use these sheets so faces stay the same.", and the link "Next: book size & layout →".
- Phone landscape / tablet: the side list and main area sit side by side from 700px wide; each scrolls on its own.
- New strings: all quoted lines above.

**Work**
- [ ] Cast agent: reads the story and lists characters and recurring places, each with a look description, as structured output; the parent can edit any description.
- [ ] Style samples: four ready-made sample images (made once, kept in `web/public/`), starting from ReadTheStupidText `prompts.md`; the chosen style id is saved with the book and used in every picture prompt.
- [ ] Sheet artist: one reference sheet per cast member and place, drawn one at a time through the picture service Slice 1 picks, saved to R2, with the prompt, style and description version kept so a later redraw and the print-quality redraw (Slice 8) can reproduce it.
- [ ] Describe screen, the progress list and the draw screen as drawn; approve, redraw with a note, go back and change words; changed words mark an approved sheet "for a redraw" and the pages that used it (Slice 8 reads the mark).
- [ ] Each draw and redraw is a CostButton (Slice 14) with `[CREDITS]` filled from Slice 11's price list; the balance check and Buy credits pop-up as in Slice 14.
- [ ] Cast eval: 20 stories with the expected characters and places labelled; measure missed and invented cast members. Pick the model (`evals/README.md`).
- [ ] Playwright: pick a style, edit a description, draw and approve all five (fake picture service), reopen one and redraw it.

**Verification**
- Cast eval table in `evals/README.md`; Playwright flow passes; sheets stay in R2 after reload.
- Changing an approved character's words shows the "marked for a redraw" state and does not charge credits until the parent presses Redraw.

**Plan changed to match the mock (2026-11-27):**
- Cast is its own step before Size & layout and Pictures; the plan had it as a "Cast screen" inside the illustrator slice (Slice 8).
- Sheets are drawn one at a time and each is approved before the next; the plan said "shown for approval; Redraw per sheet".
- Sheets cost credits and show their price; the plan did not say.
