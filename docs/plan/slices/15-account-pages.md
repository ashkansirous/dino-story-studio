### Slice 15 — Account: sign in, welcome, account, download my data, delete account

*Planned.* **Starts after:** Slice 14 for the screens; its sign-in service is the one Slice 3 picks.

**Touches:** `web/src/features/account/` (new), `api/src/studio/routes/account.py` (new), `api/src/studio/account/` (new), `docs/design/` (read only)

**Why.** Owner brief (2026-11-27): "Account: Sign in (Google, Apple, Facebook) → Welcome → Credits & payments (...) → Account → Delete account." Slice 3 builds the sign-in itself (token check, user row); this slice builds the four account screens around it and the data behind them. The Credits & payments screen belongs to Slice 11, which owns the ledger and Stripe.

**Decisions**
- A child never has an account: "Dino Story Studio is for grown-ups making books for children. Children don't need an account." The Welcome screen asks for a tick "I'm 18 or over." before the first book. Real names of the people in a book (Slice 16) stay in the book.
- Delete account removes the account, every book and all pictures for good, and the names and details added for the people in the books. Stripe keeps its own record of past payments (the mock says so on the screen).
- Unused credits at deletion: the refund rule is a placeholder `[REFUND POLICY FOR UNUSED CREDITS]` until the owner gives it. Do not invent one.
- Terms and Privacy policy are links with placeholder targets (`[Terms]`, `[Privacy policy]`) until the owner provides the pages.

#### Design (Account & Payments mock, sections 1, 2, 4 and 5, with Brand & Sign-in Art, direction A and picture 6, added 2026-11-27)

Five account boards (1 sign-in, 2 Welcome, 3 Credits & payments, 4 Account, 5 Delete account) at desktop and four device sizes; this slice covers boards 1, 2, 4 and 5. The mock disagrees with the plan on the sign-in methods (Google, Apple and Facebook, no email); that is changed below.

- **Sign in** (`account/Main.dc.html`, font Andika, text `--text`): a two-part page. Left, the picture `docs/design/assets/signin-family.jpg` (flex 1 1 360px, min-height 520px, background #DDF5E8, image `object-fit: cover`, position center 70%). Right, a column max 400px, padding 48px clamp(16px,4vw,48px): logo 48px radius 11px, "Dino" (Fredoka 700 26px, `--primary`) over "Story Studio" (Fredoka 600 22px, `--text`); h2 "Turn your story idea into a picture book" (Fredoka 600 30px/1.15); paragraph "Sign in or create an account with one tap. New accounts get free starting credits to make a first book." (16px, `--text-2`); three provider buttons, full width, min-height 52px, radius 999px, Andika 700 16px, gap 12px: "Continue with Google" (white, 2px #DADCE0 border, a round "G" mark), "Continue with Apple" (black, white text, an "A" mark), "Continue with Facebook" (#1877F2, white text, an "f" mark); then 14px `--muted` "Dino Story Studio is for grown-ups making books for children. Children don't need an account." and 13px "By continuing you agree to our Terms and Privacy policy." (links `--link`). The brand art board `brand/Signin6.dc.html` shows the same picture with other words ("Make a picture book from your own story", "Sign in to start. If you are new, signing in makes your account."); the account board is the sign-in screen, so its words are used.
  Under 700px wide or under 520px high the picture is hidden and the form moves to the top (padding 20px 16px). Phone landscape: form in two columns (max 760px, gap 32px): text left, the three buttons right; h2 24px; buttons min-height 46px.
- **Welcome** (`account/Welcome.dc.html`, first sign-in only): logo and "Dino Story Studio"; h1 "Welcome!"; "Just two things before your first book."; field "What should we call you?" prefilled from the provider's name, with the note "From your Google account. Used for "by …" on your covers; you can change it on each book." (the provider name in the note follows the provider used); checkbox 22px "**I'm 18 or over.** Accounts are for grown-ups making books for children." (required); checkbox "Email me now and then about new features. (Optional)" (unticked); a green notice "**[N] free credits** are waiting: enough for a first small book with draft pictures."; button "Start my first book", full width, min-height 52px, radius 999px, Nunito 800 16px; disabled (#E9E1D3, `cursor: not-allowed`) until "I'm 18 or over." is ticked, then `--primary`; below it "Not you? Use a different account" (link back to sign-in). Phone landscape: the card runs in two columns (max 800px).
- **Account** (`account/Account.dc.html`): header with logo, "Dino Story Studio", nav "My books", "Credits", "Account", and the credit pill linking to Credits. h1 "Account". Sections, each with an h2: "You" (field "Name"; "Email" shown as text with the tag "from Google", not editable; checkbox "Email me now and then about new features"); "How you sign in" ("Link more than one, so you can still get in if you lose access to one."; rows Google "Linked", Apple "Link", Facebook "Link"); "Your data" ("We keep your books (story text, pictures, PDFs) and the names you gave the people in them, so you can come back and keep working. They're only used to make your books." and a button "Download all my data"); "Sign out" ("On this device." and a button "Sign out"); "Delete account" ("Removes your account, every book and all pictures for good." and a link "Delete account…").
- **Delete account** (`account/DeleteAccount.dc.html`, a dialog over the Account page): title "Delete your account?"; "This can't be undone. We'll delete:" then three bullets "Your 3 books: story text, pictures, covers and PDFs", "The names and details you added for the people in your books", "Your 120 unused credits. [REFUND POLICY FOR UNUSED CREDITS]" (the 3 and 120 are the person's real numbers); "Stripe keeps its own record of your past payments. Want a copy of your books first? Download all my data"; field "Type DELETE to confirm" (case-insensitive, trimmed); button "Delete my account forever" (background #7A2E1F, white text, disabled #F3DCD6 on #8E5A50 until DELETE is typed); link "Keep my account".
- Responsive: the same boards at 390×844, 844×390, 820×1180 and 1180×820; all five account screens work in phone portrait.
- New strings, one locale file per language the site ships: every quoted line above.

**Work**
- [ ] Sign-in screen with the three provider buttons, wired to the service Slice 3 picks; first sign-in goes to Welcome, later ones to My books.
- [ ] Welcome screen: name, the 18+ tick (stored with a time on the user row), the optional newsletter tick (stored; no mail is sent by this slice), the starting-credits notice (the grant itself is Slice 11).
- [ ] Account screen: edit name, newsletter tick, link and unlink providers (the last provider cannot be unlinked), sign out.
- [ ] "Download all my data": a zip of every book (story text JSON, pictures, PDFs) and the people names, built as a background job with a loading state and a download link.
- [ ] Delete account: removes the user, books, pictures in R2, people names and the sign-in link; keeps the ledger rows and Stripe ids needed for tax and refund records, cut loose from the person (the owner confirms the exact retention in the privacy page text, placeholder `[Privacy policy]`).
- [ ] Playwright: sign in with a fake provider, Welcome blocked until 18+ is ticked, link a second provider, download data, delete with the typed word, and the books are gone.

**Verification**
- Welcome cannot be passed without the 18+ tick; the delete button stays disabled until DELETE is typed.
- After deletion the user's books and R2 files are gone (checked in the test database and a test bucket).
- Screenshots of the four screens at the five sizes are in the PR.

**Plan changed to match the mock (2026-11-27):**
- Sign-in methods: Slice 3 and plan.md Decision 1 said "email or Google"; the mock draws Google, Apple and Facebook only (the mock supersedes this).
- New screens the plan did not have: Welcome (18+ tick, optional newsletter, starting-credits notice), Account (link providers, download my data, sign out), Delete account.
