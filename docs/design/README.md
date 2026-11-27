# Dino Story Studio designs

Copied on 2026-11-27 from three Claude Design artifacts the owner approved. These copies are the source of truth for layout, copy and behaviour. Read visual values from here; never guess them and never fetch them again from the artifact while building.

| Folder | Source | Artifact version copied |
|---|---|---|
| `app/` | [Dino Story Studio — App Design](https://claude.ai/artifact/CgAcsGhtCvJtjUaxtLhMuV) (Desktop page + Phone & tablet page) | 1795742079-4dfc |
| `account/` | [Account & Payments](https://claude.ai/artifact/N34CoHnoRbNiCTZithygvE) | 1795742081-ccfb |
| `brand/` | [Brand & Sign-in Art](https://claude.ai/artifact/G5MCs8svuYKpzkMgiwhW7f) | 1795741493-7884 |

**Chosen:** brand direction A (`brand/Main.dc.html`, "Match the Dino app") and sign-in picture 6 (`brand/Signin6.dc.html`, "Mum, dad and child making their book"). `brand/BrandB.dc.html`, `brand/BrandC.dc.html`, `brand/Logos2.dc.html` and `brand/Art6.dc.html` are the options that were *not* chosen, kept for reference only. The other sign-in and art boards were not copied.

## Assets

The `.dc.html` files reference images by artifact blob URL. The two the app uses are saved here:

| File | Blob ids in the mocks | What |
|---|---|---|
| `assets/logoA.png` | `/_blob/39e5942290a1f288940ae460d77badc0` (app), `/_blob/add6022ee0364e9dfc71e34e1154fb31` (account), `/_blob/cf7c57d6a16227498f3f7504614ace2b` (brand) | Logo: the Dino drawing in a sketchbook (sha256 2d45d374…) |
| `assets/signin-family.jpg` | `/_blob/cd12f6e28642821105f93766b2bc5806` (account), `/_blob/62eaad335353df39c11896d4d1acf201` (brand) | Sign-in picture 6 (sha256 f5fb1735…) |

Other blob images in `brand/` (Dino app icon today, favicon, unchosen logos) were not copied.

## How to read the files

- Each `<Name>.dc.html` is one board: HTML with inline styles inside `<x-dc>`, a `<helmet>` with fonts and responsive CSS, and a `class Component` script whose `renderVals()` holds the sample data and interactive state. Read markup, inline styles, `@media` rules and the sample data; do not copy the code.
- `PhoneP-`, `PhoneL-`, `TabP-`, `TabL-` files are thin wrappers that render the desktop board at 390×844, 844×390, 820×1180 and 1180×820. The responsive behaviour lives in the `@media` rules of the desktop board.
- `canvas.json` in each folder lists every board, its title and its size.
- `support.js` and the artifact runtime only render the mock in Claude Design and were not copied.

## How to refresh

Read each artifact's files with the Artifact tool (`action: "read"`, `paths: ["project/..."]`), read each asset by its blob id, and overwrite the copies here. Never hand-patch a copy. Note the new artifact version in the table above.

## Boards

### `app/` — Dino Story Studio — App Design

| # | Board | File | Canvas page | Size | Lines |
|---|---|---|---|---|---|
| 1 | 1 · My books | `app/Main.dc.html` | desktop | 1280×960 | 110 |
| 2 | 2 · New book: your idea (age, pages) | `app/NewBook.dc.html` | desktop | 1280×1000 | 126 |
| 3 | 3 · New book: who it is for (optional) | `app/Audience.dc.html` | desktop | 1280×1250 | 121 |
| 4 | 4 · Shape your story (only when the idea is thin) | `app/StoryChat.dc.html` | desktop | 1280×1300 | 172 |
| 5 | 5 · Page editor + AI chat + safety | `app/Editor.dc.html` | desktop | 1280×960 | 137 |
| 6 | 6 · Cast: style, describe, draw one by one | `app/Cast.dc.html` | desktop | 1280×960 | 194 |
| 7 | 7 · Book size and where the words go (before pictures) | `app/Layout.dc.html` | desktop | 1280×960 | 160 |
| 8 | 8 · Pictures appearing, credits | `app/Pictures.dc.html` | desktop | 1280×1120 | 170 |
| 9 | 9 · Text effects (R1–R10 from print-the-damn-book) | `app/Format.dc.html` | desktop | 1280×960 | 192 |
| 10 | 10 · Front cover: prompt, draw, drag title and name | `app/CoverFront.dc.html` | desktop | 1280×1000 | 155 |
| 11 | 11 · Back cover: blurb, colour or drawn picture | `app/CoverBack.dc.html` | desktop | 1280×1000 | 146 |
| 12 | 12 · Extra pages | `app/Covers.dc.html` | desktop | 1280×1000 | 94 |
| 13 | 13 · Preview on the web, free download, print PDF for credits | `app/Preview.dc.html` | desktop | 1280×1000 | 157 |
| 14 | 1 · My books | `app/PhoneP-Main.dc.html` | devices | 390×844 | 26 |
| 15 | 2 · Your idea | `app/PhoneP-NewBook.dc.html` | devices | 390×844 | 26 |
| 16 | 3 · Who it is for | `app/PhoneP-Audience.dc.html` | devices | 390×844 | 26 |
| 17 | 4 · Shape your story | `app/PhoneP-StoryChat.dc.html` | devices | 390×844 | 26 |
| 18 | 5 · Page editor | `app/PhoneP-Editor.dc.html` | devices | 390×844 | 26 |
| 19 | 6 · Cast | `app/PhoneP-Cast.dc.html` | devices | 390×844 | 26 |
| 20 | 7 · Size & layout | `app/PhoneP-Layout.dc.html` | devices | 390×844 | 26 |
| 21 | 8 · Pictures | `app/PhoneP-Pictures.dc.html` | devices | 390×844 | 26 |
| 22 | 9 · Text effects | `app/PhoneP-Format.dc.html` | devices | 390×844 | 26 |
| 23 | 10 · Front cover | `app/PhoneP-CoverFront.dc.html` | devices | 390×844 | 26 |
| 24 | 11 · Back cover | `app/PhoneP-CoverBack.dc.html` | devices | 390×844 | 26 |
| 25 | 12 · Extra pages | `app/PhoneP-Covers.dc.html` | devices | 390×844 | 26 |
| 26 | 13 · Preview & download | `app/PhoneP-Preview.dc.html` | devices | 390×844 | 26 |
| 27 | 1 · My books | `app/PhoneL-Main.dc.html` | devices | 844×390 | 26 |
| 28 | 2 · Your idea | `app/PhoneL-NewBook.dc.html` | devices | 844×390 | 26 |
| 29 | 3 · Who it is for | `app/PhoneL-Audience.dc.html` | devices | 844×390 | 26 |
| 30 | 4 · Shape your story | `app/PhoneL-StoryChat.dc.html` | devices | 844×390 | 26 |
| 31 | 5 · Page editor | `app/PhoneL-Editor.dc.html` | devices | 844×390 | 26 |
| 32 | 6 · Cast | `app/PhoneL-Cast.dc.html` | devices | 844×390 | 26 |
| 33 | 7 · Size & layout | `app/PhoneL-Layout.dc.html` | devices | 844×390 | 26 |
| 34 | 8 · Pictures | `app/PhoneL-Pictures.dc.html` | devices | 844×390 | 26 |
| 35 | 9 · Text effects | `app/PhoneL-Format.dc.html` | devices | 844×390 | 26 |
| 36 | 10 · Front cover | `app/PhoneL-CoverFront.dc.html` | devices | 844×390 | 26 |
| 37 | 11 · Back cover | `app/PhoneL-CoverBack.dc.html` | devices | 844×390 | 26 |
| 38 | 12 · Extra pages | `app/PhoneL-Covers.dc.html` | devices | 844×390 | 26 |
| 39 | 13 · Preview & download | `app/PhoneL-Preview.dc.html` | devices | 844×390 | 26 |
| 40 | 1 · My books | `app/TabP-Main.dc.html` | devices | 820×1180 | 26 |
| 41 | 2 · Your idea | `app/TabP-NewBook.dc.html` | devices | 820×1180 | 26 |
| 42 | 3 · Who it is for | `app/TabP-Audience.dc.html` | devices | 820×1180 | 26 |
| 43 | 4 · Shape your story | `app/TabP-StoryChat.dc.html` | devices | 820×1180 | 26 |
| 44 | 5 · Page editor | `app/TabP-Editor.dc.html` | devices | 820×1180 | 26 |
| 45 | 6 · Cast | `app/TabP-Cast.dc.html` | devices | 820×1180 | 26 |
| 46 | 7 · Size & layout | `app/TabP-Layout.dc.html` | devices | 820×1180 | 26 |
| 47 | 8 · Pictures | `app/TabP-Pictures.dc.html` | devices | 820×1180 | 26 |
| 48 | 9 · Text effects | `app/TabP-Format.dc.html` | devices | 820×1180 | 26 |
| 49 | 10 · Front cover | `app/TabP-CoverFront.dc.html` | devices | 820×1180 | 26 |
| 50 | 11 · Back cover | `app/TabP-CoverBack.dc.html` | devices | 820×1180 | 26 |
| 51 | 12 · Extra pages | `app/TabP-Covers.dc.html` | devices | 820×1180 | 26 |
| 52 | 13 · Preview & download | `app/TabP-Preview.dc.html` | devices | 820×1180 | 26 |
| 53 | 1 · My books | `app/TabL-Main.dc.html` | devices | 1180×820 | 26 |
| 54 | 2 · Your idea | `app/TabL-NewBook.dc.html` | devices | 1180×820 | 26 |
| 55 | 3 · Who it is for | `app/TabL-Audience.dc.html` | devices | 1180×820 | 26 |
| 56 | 4 · Shape your story | `app/TabL-StoryChat.dc.html` | devices | 1180×820 | 26 |
| 57 | 5 · Page editor | `app/TabL-Editor.dc.html` | devices | 1180×820 | 26 |
| 58 | 6 · Cast | `app/TabL-Cast.dc.html` | devices | 1180×820 | 26 |
| 59 | 7 · Size & layout | `app/TabL-Layout.dc.html` | devices | 1180×820 | 26 |
| 60 | 8 · Pictures | `app/TabL-Pictures.dc.html` | devices | 1180×820 | 26 |
| 61 | 9 · Text effects | `app/TabL-Format.dc.html` | devices | 1180×820 | 26 |
| 62 | 10 · Front cover | `app/TabL-CoverFront.dc.html` | devices | 1180×820 | 26 |
| 63 | 11 · Back cover | `app/TabL-CoverBack.dc.html` | devices | 1180×820 | 26 |
| 64 | 12 · Extra pages | `app/TabL-Covers.dc.html` | devices | 1180×820 | 26 |
| 65 | 13 · Preview & download | `app/TabL-Preview.dc.html` | devices | 1180×820 | 26 |
| 66 | 14 · Buy credits pop-up (opens over any screen) | `app/BuyCredits.dc.html` | desktop | 1280×900 | 69 |
| 67 | 14 · Buy credits pop-up | `app/PhoneL-BuyCredits.dc.html` | devices | 844×390 | 26 |

### `account/` — Dino Story Studio — Account & Payments

| # | Board | File | Canvas page | Size | Lines |
|---|---|---|---|---|---|
| 1 | 1 · Sign in or create an account | `account/Main.dc.html` | desktop | 1280×860 | 50 |
| 2 | 2 · First sign-in: welcome | `account/Welcome.dc.html` | desktop | 1280×860 | 66 |
| 3 | 3 · Credits & payments | `account/Credits.dc.html` | desktop | 1280×1100 | 116 |
| 4 | 4 · Account | `account/Account.dc.html` | desktop | 1280×1100 | 79 |
| 5 | 5 · Delete account | `account/DeleteAccount.dc.html` | desktop | 1280×1100 | 58 |
| 6 | 1 · Sign in | `account/PhoneP-Main.dc.html` | devices | 390×844 | 26 |
| 7 | 2 · Welcome | `account/PhoneP-Welcome.dc.html` | devices | 390×844 | 26 |
| 8 | 3 · Credits & payments | `account/PhoneP-Credits.dc.html` | devices | 390×844 | 26 |
| 9 | 4 · Account | `account/PhoneP-Account.dc.html` | devices | 390×844 | 26 |
| 10 | 5 · Delete account | `account/PhoneP-DeleteAccount.dc.html` | devices | 390×844 | 26 |
| 11 | 1 · Sign in | `account/PhoneL-Main.dc.html` | devices | 844×390 | 26 |
| 12 | 2 · Welcome | `account/PhoneL-Welcome.dc.html` | devices | 844×390 | 26 |
| 13 | 3 · Credits & payments | `account/PhoneL-Credits.dc.html` | devices | 844×390 | 26 |
| 14 | 4 · Account | `account/PhoneL-Account.dc.html` | devices | 844×390 | 26 |
| 15 | 5 · Delete account | `account/PhoneL-DeleteAccount.dc.html` | devices | 844×390 | 26 |
| 16 | 1 · Sign in | `account/TabP-Main.dc.html` | devices | 820×1180 | 26 |
| 17 | 2 · Welcome | `account/TabP-Welcome.dc.html` | devices | 820×1180 | 26 |
| 18 | 3 · Credits & payments | `account/TabP-Credits.dc.html` | devices | 820×1180 | 26 |
| 19 | 4 · Account | `account/TabP-Account.dc.html` | devices | 820×1180 | 26 |
| 20 | 5 · Delete account | `account/TabP-DeleteAccount.dc.html` | devices | 820×1180 | 26 |
| 21 | 1 · Sign in | `account/TabL-Main.dc.html` | devices | 1180×820 | 26 |
| 22 | 2 · Welcome | `account/TabL-Welcome.dc.html` | devices | 1180×820 | 26 |
| 23 | 3 · Credits & payments | `account/TabL-Credits.dc.html` | devices | 1180×820 | 26 |
| 24 | 4 · Account | `account/TabL-Account.dc.html` | devices | 1180×820 | 26 |
| 25 | 5 · Delete account | `account/TabL-DeleteAccount.dc.html` | devices | 1180×820 | 26 |

### `brand/` — Dino Story Studio — Brand & Sign-in Art

| # | Board | File | Canvas page | Size | Lines |
|---|---|---|---|---|---|
| 1 | A · Match the Dino app | `brand/Main.dc.html` | brand | 1280×900 | 69 |
| 2 | B · Own look, Dino family | `brand/BrandB.dc.html` | brand | 1280×900 | 68 |
| 3 | C · Completely fresh | `brand/BrandC.dc.html` | brand | 1280×900 | 68 |
| 9 | 6 · Your bookshelf | `brand/Art6.dc.html` | art | 1280×800 | 65 |
| 10 | Five new logos (made in Gemini) | `brand/Logos2.dc.html` | logos2 | 1600×830 | 79 |
| 16 | 6 · Mum, dad and child making their book (3 + 5) | `brand/Signin6.dc.html` | signin3 | 1280×800 | 40 |

## The owner's prompt (verbatim, 2026-11-27)

The owner's message was "update the plan with the design", with this brief pasted:

```text
Build the web front end (React + TypeScript) for every page of Dino Story Studio, matching the approved designs. Follow CLAUDE.md, plan.md and docs/plan/slices/. Start in plan mode: show me the component and route plan before writing code.

## Design source
- App screens: https://claude.ai/artifact/CgAcsGhtCvJtjUaxtLhMuV ("Dino Story Studio — App Design": Desktop page + "Phone & tablet" page)
- Account screens: https://claude.ai/artifact/N34CoHnoRbNiCTZithygvE ("Account & Payments")
- Brand and sign-in art: https://claude.ai/artifact/G5MCs8svuYKpzkMgiwhW7f (we chose direction A and sign-in picture 6)
- The design source files (.dc.html) and images are in docs/design/. Treat them as the reference for layout, copy and behaviour, not as code to copy. Where the design and the plan disagree, the design wins; list every disagreement for me at the end.

## Brand (direction A, matches the Dino app)
Put these in one theme file as CSS variables, never hard-coded:
- background #FFF6EE, card #FFFCF9, border #F1DCCB, soft fill #FBEDE2
- primary (buttons, accent text) #2E7D32, hover #1B5E20, light tint #E3F5E5
- text #2b2540, muted #5d5672, links #5B57E8 (hover #3f3bc4)
- accents from the Dino app: green #4FC060 (art only), orange #FFA94D, pink #FF7A9C
- warnings keep the orange set from the design (#FFF1E4 / #8A4A14)
- fonts: Fredoka for headings, Nunito for body. The sign-in screen uses Andika. Make the body font a single token so we can switch everything to Andika later in one place.
- logo: docs/design/assets/logoA.png (the Dino drawing in a sketchbook). Sign-in picture: docs/design/assets/signin-family.jpg

## Pages
App, in flow order: My books → New book (your idea, age, pages) → Who it's for (optional) → Shape your story (story agent talks first, asks questions, suggests options) → Story editor → Cast (style samples → descriptions → each character drawn one at a time) → Size & layout → Pictures → Text effects (rules R1–R10, editable speaker colours) → Front cover → Back cover → Extra pages → Preview & download.
Account: Sign in (Google, Apple, Facebook) → Welcome → Credits & payments (balance, buy, credit-use history, payment history with Stripe receipt links; no billing page) → Account → Delete account.
Overlay: Buy credits pop-up, which can open over any screen.

## Rules from the design that must hold
- Every step can always be reopened from the step bar, and nothing is locked after "approval". The step bar order is: Story, Cast, Size & layout, Pictures, Text effects, Front cover, Back cover, Extra pages, Preview & download.
- The credit balance is on every screen. Every paid button shows its cost. Running out opens the Buy credits pop-up over the current screen.
- Pictures are always cheap drafts: no quality picker and no accept step. Clicking a picture opens a chat with two actions: "Change this picture" or "Draw a new picture", with the prompt shown and editable. The full-quality redraw happens only when the print PDF is bought.
- Covers and extra pages are never drawn automatically. Show an editable suggested prompt and wait for Draw. Text boxes (title, author) can be dragged on a large preview.
- Size & layout comes before pictures. The "text on the page (30% band)" option exists only for portrait pages, and only when the text fits.
- Preview & download has a free preview PDF and a print-quality PDF for credits. It has no rebuild button.
- Shown prices, credit amounts, refund rules and similar stay as placeholders ([PRICE], [CREDITS], [N]). Never invent them.

## Responsive (check every page at each size)
- Phone portrait (390×844): only My books works. Every book screen shows "Turn your phone sideways".
- Phone landscape (844×390, max-height 520px): compact header, content first, side-by-side panes that each scroll on their own.
- Tablet portrait (820×1180): side panels stay side by side. Panes stack only below 700px wide.
- Tablet landscape (1180×820) and desktop (1280+).
- Text areas grow with their content (field-sizing: content, with a JS fallback) and never show their own scrollbars. Long option lists scroll inside their pane, so the page preview stays visible.

## How to work
- Build shared pieces first: AppHeader, StepBar, CreditPill, CostButton, BuyCreditsModal, PageShell, and the theme tokens. Then build the pages in the order above, one slice at a time.
- Use mock data and fake API responses only. No real Claude, picture or Stripe calls yet.
- After each page, take Playwright screenshots at the 5 sizes above, look at them yourself, and fix overflow, clipping, scrollbars and overlap before moving on. Keep the screenshots in the PR.
- Accessibility: real buttons, links and labels; text contrast at least 4.5:1; touch targets at least 44px.
- Open one PR per group of pages. Never deploy from a shell: releases go only through the GitHub Actions workflow on merge to main.
```
