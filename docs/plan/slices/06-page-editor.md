### Slice 6 — Page editor: edit by hand, chat with AI per page, add, remove and move pages

*Planned.* **Starts after:** Slice 3.

**Touches:** `web/src/features/editor/` (new), `api/src/studio/routes/pages.py` (new), `api/src/studio/agents/page_chat/` (new), `evals/datasets/page_chat/` (new)

**Why.** The owner's flow: "they review it and edit whatever they want. on each page, they can decide also to talk to AI and chat to change
the text on that page or manually, add/remove or move a page" (owner, 2026-11-26).

**Work**
- [ ] Page list with each page's text, editable in place; saves on blur with a "Saved" mark; undo for the last change.
- [ ] Add a page (blank or "AI, write a page here" from the pages around it), delete a page (with undo), drag to reorder.
- [ ] Per-page AI chat: a side panel where the parent asks for a change ("make it funnier", "the dog should say hello"); the agent sees the whole story
      but returns only the new text for that page, shown as a before/after the parent accepts or rejects.
- [ ] The age group's words-per-page limit shows live under each page; going over shows a warning, not a block.
- [ ] Every saved change runs the safety check (Slice 5, once built; until then the hook point is left in place).
- [ ] Page-chat eval: 25 page + request pairs; checks that only the asked change happened, other pages are untouched, and the limit is kept. Pick the model from it.
- [ ] Any AI call over 500 ms shows a loading state with Cancel.

**Verification**
- Playwright: edit, add, delete, move and an accepted AI chat change all survive a page reload.
- Page-chat eval table and model pick in `evals/README.md`.
