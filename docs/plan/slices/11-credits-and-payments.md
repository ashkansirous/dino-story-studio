### Slice 11 — Credits, free starting credits, Stripe payments and the Credits & payments screen

*Planned.* **Starts after:** Slices 8 and 14. (Slices 17 and 18 show prices as placeholders until this slice's price list exists.)

**Touches:** `api/src/studio/billing/` (new), `api/src/studio/routes/billing.py` (new), `web/src/features/billing/` (new)

**Why.** Decision 1 (owner, 2026-11-26): writing and editing are free; pictures and the finished book cost credits, bought by card through Stripe.
"I am also thinking of giving some credits, like opening token. that can be enough for a small book with low quality images."
Owner brief (2026-11-27): "Credits & payments (balance, buy, credit-use history, payment history with Stripe receipt links; no billing page)"; "The credit balance is on every screen. Every paid button shows its cost. Running out opens the Buy credits pop-up over the current screen."

**Decisions**
- New accounts get free starting credits worth one 12-page book at "draft" picture quality (priced from the Slice 1 note and the real cost log from Slice 3). The screens say "[N] free credits" (Welcome) and "Enough for one 12-page book with draft-quality pictures." (My books) until the owner fixes the number. The grant is given when the Welcome screen is finished (Slice 15), once.
- Credits are spent on: cast sheets (draw and redraw, Slice 18), page draft pictures and changes to a picture (Slice 8), the front cover picture and the back cover picture (Slice 9), and the print-quality PDF (Slice 17). Free: writing, editing and chat on text, style samples, cast descriptions, text effects, extra pages, a colour back cover, the free preview PDF, going back to an earlier picture version. Text AI calls are free but rate-limited per account.
- Credit packs: three, "Small", "Medium" and "Large", each "[N] credits" for "[PRICE]" (placeholders until the owner gives them; never invented). Bought with Stripe Checkout; credits are added only by the verified Stripe webhook, never by the browser's return page.
- After paying the parent comes straight back to the screen they were on (the pop-up opens over any screen, so the Checkout return address is that screen), and the unfinished action can be done again.
- There is no billing page and no stored card: the parent pays on Stripe's page ("we never see your card"), a receipt is emailed, and the payments table links to the Stripe receipt.
- Refunds: a payment can show "Refunded". The refund rule for unused credits at account deletion is the owner's placeholder `[REFUND POLICY FOR UNUSED CREDITS]` (Slice 15).

#### Design (Account & Payments mock, section 3 "Credits & payments" (board 3; devices 8, 13, 18, 23), and App Design mock, section 14 "Buy credits pop-up" (boards 66-67), added 2026-11-27)

Two desktop boards plus four device sizes of the first and a phone-landscape board of the pop-up. The mock agrees with the plan about credits and Stripe; it adds the history screen and the pop-up, and the list of what costs credits (above).

- **Credits & payments** (`account/Credits.dc.html`; works in phone portrait): header with logo, "Dino Story Studio", nav "My books", "Credits" (current), "Account", and the credit pill "120 credits" (links to this screen). h1 "Credits & payments"; "Writing and editing are free. Credits pay for pictures and the print-quality PDF."; a balance card: "YOUR BALANCE" (small caps), "120" large, "credits left"; h2 "Buy credits" with the pack group "Credit pack" (three radio cards: name, "[N] credits", "[PRICE]"; Medium selected) and the button "Pay with card" and the line "You pay on Stripe's secure page; we never see your card. A receipt is emailed to you."; then two tabs (aria-label "History", pills min-height 40px): "Credit use" and "Payments".
  "Credit use" table: columns "Date", "Book", "What", "Credits" (right-aligned, weight 800; additions "+[N]" in #1B5E20, spends "−[N]"). Rows in the mock: Today, "The Hen Who Shared Her Bread", "Changed picture · page 5"; "Draft pictures · 12 pages"; "Cast sheet · The Cat (redraw)"; "Cast sheets · 5"; 24 Nov, "—", "Bought Medium pack"; 3 Nov, "Three Little Pigs Go Camping", "Print-quality PDF"; 1 Nov, "—", "Starting credits".
  "Payments" table: columns "Date", "Pack", "Paid", "Status", "Receipt". Rows: "24 Nov 2026", "Medium · [N] credits", "[PRICE]", a "Paid" tag, link "View receipt"; "2 Nov 2026", "Small · [N] credits", "[PRICE]", a "Refunded" tag, "View receipt". Footer: "Receipts open on Stripe. Need help with a payment? Contact us." (the "Contact us" target is a placeholder until the owner gives a contact).
- **Buy credits pop-up:** as Slice 14 describes (its text and sizes).
- New strings: all quoted lines above.

**Work**
- [ ] Credit ledger in Postgres (every grant, purchase and spend is a row with the date, the book, a label such as "Cast sheet · The Cat (redraw)" and the amount; the balance is their sum). The "Credit use" table reads it.
- [ ] Price list from real costs: cost per cast sheet, per draft picture, per picture change, per cover picture and for the print-quality PDF from the call log, with a margin; written to `docs/plan/notes/` with the numbers. The screens keep the placeholders `[CREDITS]`, `[CREDITS PER SHEET]`, `[DRAFT CREDITS]`, `[N]`, `[PRICE]` until the owner approves the list.
- [ ] Starting credits on finishing Welcome; one grant per person: by verified email and by provider account (Apple may hide the email; check the provider docs with `library-docs` and record what can and cannot be matched).
- [ ] Stripe Checkout for 3 credit packs; webhook adds credits once per event (idempotent); test mode first; the payments table (date, pack, amount, status, receipt URL) filled from the webhook; refunds update the status.
- [ ] Credits & payments screen as drawn, and the Buy credits pop-up wired to Checkout with the return to the current screen.
- [ ] Every CostButton checks the balance before it starts; not enough credits opens the pop-up in its "Not enough credits" state; before a picture job starts, show its credit cost and the balance.
- [ ] Rate limit for free text AI calls per account per day.

**Verification**
- Stripe test mode: buy a pack, the webhook adds credits once even if Stripe sends it twice; the payments table shows "Paid" with a working receipt link; a refunded payment shows "Refunded".
- A new account can make one 12-page draft book with its starting credits and is asked to buy for a second.
- With 1 credit, a cast sheet draw opens the pop-up and charges nothing; after a test purchase the parent is back on the same screen.
- Screenshots of the screen and the pop-up at the five sizes are in the PR.

**Plan changed to match the mock (2026-11-27):**
- Credits are also spent on cast sheets, cover pictures and picture changes (the plan listed pictures and the print PDF; the mock adds them).
- New screen: Credits & payments with the balance, packs, credit-use history, payment history and Stripe receipt links; no billing page.
- Grant timing: at the end of Welcome, not at sign-up.
