### Slice 6 — Page editor: edit by hand, chat with AI per page, add, remove and move pages

*Planned.* **Starts after:** Slices 3 and 14.

**Touches:** `web/src/features/editor/` (new), `api/src/studio/routes/pages.py` (new), `api/src/studio/agents/page_chat/` (new), `evals/datasets/page_chat/` (new)

**Why.** The owner's flow: "they review it and edit whatever they want. on each page, they can decide also to talk to AI and chat to change
the text on that page or manually, add/remove or move a page" (owner, 2026-11-26).

#### Design (App Design mock, section 5 "Page editor + AI chat + safety" (board 5; devices 18, 31, 44, 57), added 2026-11-27)

One desktop board and its four device sizes. The mock agrees with the plan: edit in place, per-page chat with before/after, add blank or AI page, delete with undo, drag to reorder, words-per-page counter that warns and does not block. It adds the chat panel layout, the "Saved" mark, an Undo button and the link to the next step.

- Book header: logo link "My books", title "The Hen Who Shared Her Bread" (Fredoka 600 20px), "Ages 4–6 · 12 pages · " and a "Saved" mark (shown after each save), the step bar with "1 Story" current (Slice 14), the credit pill.
- Layout "split": the page list in the main column, the chat in a right side pane (`<aside aria-label="AI chat for page 2">`). Tablet portrait keeps them side by side (3 to 2); under 700px the chat stacks under the list; in phone landscape each pane scrolls alone.
- Page list header: h1 "Pages", a button "Undo", and the link "Next: meet the cast →".
- Each page is an article (card, radius 18px): a drag handle button (aria-label "Drag to move page 1", 44px), "Page 1", the live counter "24 / 30 words" (the limit is the age group's words per page: 30 for ages 4–6), and a text area with the text (Nunito 18px/1.55, no border, grows with its content, no scrollbar; aria-label "Page 1 text"). A page being chatted about shows "Page 2 · chatting". Over the limit: "36 / 30 words · a bit long", in the warning colours; it warns and does not block.
- A safety finding sits inside the page it is about (Slice 5): "House rule: nobody gets hurt", "The Duck bumps her head. Try a fall with no hurt.", "Fix with AI", "Keep anyway".
- Between pages: "+ Blank page here" and "+ AI, write a page here". After the visible pages: "Pages 4–12 continue below · drag to reorder · delete from the page menu (with undo)". The page menu holds Delete (with undo).
- Chat pane: title "Chat about page 2", subtitle "Only this page changes", a close button (aria-label "Close chat", 44px); the parent's message ("Make it funnier, and the dog should yawn."); the answer as a before/after card: "BEFORE" `"Not I," said the Cat. "Not I," said the Dog.` and "AFTER" `"Not I," purred the Cat, licking her paw. "Not I-YAWN," said the Dog, already half asleep.` with the buttons "Accept" and "Reject"; the field "Ask for a change" (placeholder "Ask for a change to this page…") and a send button (aria-label "Send").
- Not drawn, carried from the plan: the loading state with Cancel for any AI call over 500 ms; a "Saved" fail state.
- New strings: all quoted lines above.

**Work**
- [ ] Page list with each page's text, editable in place; saves on blur with a "Saved" mark in the book header; the Undo button undoes the last change.
- [ ] Add a page (blank or "AI, write a page here" from the pages around it), delete a page from the page menu (with undo), drag to reorder (the handle is also operable with the keyboard).
- [ ] Per-page AI chat: a side pane where the parent asks for a change ("make it funnier", "the dog should say hello"); the agent sees the whole story
      but returns only the new text for that page, shown as BEFORE / AFTER that the parent accepts or rejects.
- [ ] The age group's words-per-page limit shows live on each page as "n / limit words"; going over shows "a bit long", not a block.
- [ ] Every saved change runs the safety check (Slice 5, once built; until then the hook point is left in place).
- [ ] The step bar's "Next: meet the cast →" link and every other step stay open (Slice 14).
- [ ] Page-chat eval: 25 page + request pairs; checks that only the asked change happened, other pages are untouched, and the limit is kept. Pick the model from it.
- [ ] Any AI call over 500 ms shows a loading state with Cancel.

**Verification**
- Playwright: edit, add, delete, move and an accepted AI chat change all survive a page reload.
- Page-chat eval table and model pick in `evals/README.md`.
- Screenshots at the five sizes are in the PR.

**Plan changed to match the mock (2026-11-27):**
- Per-page chat's before/after is the mock's BEFORE / AFTER card with Accept and Reject; the plan said "before/after the parent accepts or rejects" (same, now exact).
- The editor's links: "Next: meet the cast →" leads to the Cast step (the plan had no next step; Cast is now step 2, Slice 18).
- Sibling slices: Cast (Slice 18) and Size & layout (Slice 7) both come before Pictures.
