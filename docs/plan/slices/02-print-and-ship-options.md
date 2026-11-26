### Slice 2 — Investigate: printing and posting 1-5 copies to a customer

*Planned.*

**Touches:** `docs/plan/notes/` (new)

**Why.** The first version ends with a print-ready PDF. The owner wants to know the options for people who want 1-5
printed copies delivered to their door (owner, 2026-11-26: "would amazon create them on my account and deliver them to
their address? should I use a separate print service?").

**Work**
- [ ] Amazon KDP: can author copies be ordered on the owner's account and posted to someone else's address; limits, cost per copy
      for a 24-32 page 8.5in square colour book, delivery times to the UK, US and EU; and whether KDP terms allow this as a paid service for strangers
      (each book would have to be a published or draft title on the owner's account).
- [ ] Print-on-demand services with an API that print one copy and post it to any address: at least Lulu (Print API), Gelato, Peecho and Blurb.
      For each: API (order, price quote, status webhook), supported sizes (A5, 8.5in square), paper, cost of one copy and of 5, shipping cost and time to UK/US/EU,
      file rules (bleed, colour profile, PDF/X), and whether the end customer can pay them directly.
- [ ] One table plus a recommendation in `docs/plan/notes/2026-11-xx-print-and-ship-options.md`, with a source link for every price.
- [ ] If a service wins, write the follow-up slice (order a printed copy from the app) into the plan, numbered with `parallel-check`.

**Verification**
- The note has a price for 1 and 5 copies from every option, each with a source link, read this session (`web-research`).
