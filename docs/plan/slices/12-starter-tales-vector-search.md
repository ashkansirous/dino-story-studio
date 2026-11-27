### Slice 12 — Public-domain starter tales, found with embeddings and vector search

*Planned.* **Starts after:** Slice 3.

**Touches:** `api/src/studio/tales/` (new), `api/src/studio/embeddings/` (new), `data/tales/` (new), `web/src/features/new-book/`, `evals/datasets/tale_search/` (new)

**Why.** The owner wants "an embedding and vector search" where there is a real use case (2026-11-26). A parent who types
"a brave little hen who shares her bread" should be offered matching classic tales (The Little Red Hen) as a starting point;
the original project line also says "turns a public-domain tale into" a book.

**Decisions**
- Tales are public domain only, each with its source and the reason it is public domain recorded (e.g. Grimm, Aesop, Andersen, Jacobs' English Fairy Tales).
- Vectors live in Postgres with pgvector (Slice 3), next to the tale rows.
- The embedding model is picked by the retrieval eval below, comparing at least two (e.g. one from Voyage AI, which Anthropic recommends, and one from OpenAI).

#### Design (App Design mock, boards 2-3 (New book), added 2026-11-27)

The mock does not draw "Start from a classic tale": New book step 1 (`app/NewBook.dc.html`, Slice 3) has the idea box, age group and page count only. Not drawn, so the item stays. It is built in the New book screen's own style: under the "Your idea" box, a list of up to 3 cards (radius 16px, `--card`, 2px `--border`, selected 2px `--primary` on `--primary-tint`, min-height 44px targets) with the tale's title and one line of summary, shown after the parent has typed; picking one fills the idea box. The flow after it is Slice 16's (Who it's for, then the story helper only when the idea is thin).

**Work**
- [ ] 100-200 tales: title, short summary, themes, characters, source link, written to `data/tales/` (summaries written by AI, source text kept for the writer).
- [ ] Embedding job: summary + themes embedded; re-run when the model changes (model name stored per vector).
- [ ] "Start from a classic tale" on the New book screen: as the parent types, show the 3 nearest tales; picking one fills the idea box with the retold outline.
- [ ] Hybrid search (keyword + vector) if the eval shows plain vector search misses title searches like "three pigs".
- [ ] Retrieval eval: 50 parent-style queries with the right tale labelled; measure recall at 3 and MRR per embedding model; pick and record in `evals/README.md`.

**Verification**
- Recall at 3 of at least 0.8 on the eval set with the chosen model; Playwright: type an idea, pick a suggested tale, and the story is written from it.
