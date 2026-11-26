### Slice 11 — Credits, free starting credits and Stripe payments

*Planned.* **Starts after:** Slice 8.

**Touches:** `api/src/studio/billing/` (new), `api/src/studio/routes/billing.py` (new), `web/src/features/billing/` (new)

**Why.** Decision 1 (owner, 2026-11-26): writing and editing are free; pictures and the finished book cost credits, bought by card through Stripe.
"I am also thinking of giving some credits, like opening token. that can be enough for a small book with low quality images."

**Decisions**
- New accounts get free starting credits worth one 12-page book at "draft" picture quality (priced from the Slice 1 note and the real cost log from Slice 3).
- Credits are spent on pictures (per picture, by quality and redraws) and on the print-quality PDF download. Text AI calls are free but rate-limited per account.
- Credit packs bought with Stripe Checkout; credits are added only by the verified Stripe webhook, never by the browser's return page.

**Work**
- [ ] Credit ledger in Postgres (every grant, purchase and spend is a row; the balance is their sum).
- [ ] Price list from real costs: cost per picture per quality from the call log, with a margin; written to `docs/plan/notes/` with the numbers.
- [ ] Starting credits on sign-up; one grant per person (email verified; block repeat sign-ups from the same Google account).
- [ ] Stripe Checkout for 2-3 credit packs; webhook adds credits once per event (idempotent); test mode first.
- [ ] Before a picture job starts, show its credit cost and the balance; not enough credits opens the buy screen.
- [ ] Rate limit for free text AI calls per account per day.

**Verification**
- Stripe test mode: buy a pack, the webhook adds credits once even if Stripe sends it twice.
- A new account can make one 12-page draft book with its starting credits and is asked to buy for a second.
