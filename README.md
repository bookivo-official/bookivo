# BOOKIVO

BOOKIVO is a phone-first, static ebook store built with HTML, CSS and vanilla JavaScript. Supabase Auth handles email OTP sign-up/login and the Google sign-in flow; catalog, orders, payments, Bookivo Bits, support, reviews, and several account features remain preview data until their server flows are connected.

## What was built

- A shared announcement bar, responsive header, category navigation, footer, search panel, Contact modal and Bookivo Assistant.
- The announcement bar uses an animated blue-to-navy gradient and moving shine; the search and shopping-bag cart match the supplied references.
- A data-driven catalogue with twelve separate Bachelor 99 Survival Cookbook entries, plus category/search/filter pages.
- `BOOK-1/` through `BOOK-12/` each contain a copied Cookbook page and its image assets. Each folder has its own catalog ID, cart entry and order record.
- A multi-ebook cart, OTP checkout, Cashfree checkout preview, four-state order confirmation and a pure discount engine.
- Supabase Auth email OTP sign-up/login, Google sign-in flow, account pages, owned-book shelf, reader watermark and progress, Bookivo Bits, coupons, tickets, FAQ and policy pages.
- A reusable CSS token system and components, pricing checks and backend handoff notes.
- A verified-buyer review gate, purchase modal matching the supplied layout, and a signed-in-only calculator route.

## How to run

Serve this folder with a local web server to use Supabase Auth. `file://` pages do not provide a valid auth redirect origin. Google Fonts and the pinned Supabase JavaScript client load from the internet; local system fonts are the fallback.

For example, from this folder you can run `python -m http.server 8000` if Python is already installed, then open `http://localhost:8000`. No package install or build is needed.

## Configuration and DEV mode

`supabase-config.js` holds the public Supabase Project URL and publishable key. Publishable keys are designed for browser code; never put a secret or `service_role` key in the website. `js/config.js` initializes the client, and the auth store restores the signed-in Supabase user before protected pages load.

The sign-up page from the supplied design is at `sign in/index.html`; `login.html` and `signup.html` open that page. Successful email or Google sign-in returns to the root `index.html`.

`js/config.js` also sets:

```js
window.BOOKIVO_CONFIG = { USE_MOCK: true, AUTH_PROVIDER: 'supabase', META_PIXEL_ID: '' };
```

The `dev` flag enables local draft notices. It does not bypass Supabase Auth or supply a test OTP.

## Script order

Most app pages include one classic script tag, `js/boot.js`. It loads the pinned Supabase client and public project config first, then classic app scripts sequentially. The non-landing order is:

1. Supabase UMD client, `supabase-config.js`, then `js/config.js`
2. `js/lib/storage.js`, `format.js`, `dom.js`, `track.js`, `pricing.js`, `store.js`
3. `js/data/categories.js`, `books.js`, `reviews.js`, `coupons.js`, `faq.js`, `content.js`, `legal.js`, `pages-history.js`, `disposable-domains.js`
4. `js/api/base.js`, then `auth.js`, `books.js`, `cart.js`, `orders.js`, `pages.js`, `coupons.js`, `reviews.js`, `support.js`, `chat.js`, `config.js`
5. `js/stores/auth.js`, `cart.js`, `pages.js`, `ui.js`
6. `js/components/toast.js`, `modal.js`, `rating.js`, `accordion.js`, `skeleton.js`, `bookcard.js`, `otp-input.js`, `lightbox.js`, `gallery.js`, `drawer.js`, `contact.js`, `chat.js`, `confetti.js`, `scratch-card.js`, `search.js`, `shell.js`
7. `js/pages/checkout.js`, `js/pages/runtime.js`
8. The page handler from `js/pages/` (for example, `home.js` or `book.js`)

The Road to 5Cr landing page uses a smaller loader set and does not load the Contact, chat or search modules.

## Folder map

- `css/` — tokens, base styles, shared components, shell, page layouts and landing-page styles.
- `js/lib/` — storage wrapper, formatting, DOM helpers, tracking, pricing and pub/sub store.
- `js/data/` — catalogue, reviews, FAQs, coupons, page content, legal policies and sample Pages history.
- `js/api/` — Supabase Auth adapter and browser-side preview APIs for the remaining domains.
- `js/stores/` — Supabase session-backed auth state, browser-persisted cart and Bookivo Bits preview state, plus transient UI state.
- `js/components/` — shared shell, cards, search, gallery, modal, contact, chat, OTP, scratch card and feedback components.
- `js/pages/` — page-specific behavior, including checkout and reader.
- `js/dev/` — local-only Dev Panel.
- `images/` — supplied Cookbook imagery and the supplied logo at `images/logo.png`.
- `sign in/` — supplied BOOKIVO email OTP and Google sign-in page and character image.
- `assets/author.png` — expected author photo path; add the image here.
- `Free calculater/index.html` — expected calculator page path; add the calculator page here.
- `downloads/` — sample chapter and related supplied download files.
- `tests/` — browser and Node pricing checks.
- `docs/` — QA checklist and backend handoff.

## Backend handoff

Supabase Auth now handles email OTP sign-in and signup, session persistence, sign-out, and the Google OAuth call. The Google provider still needs a Google OAuth Client ID and secret entered in Supabase Auth settings. Supabase built-in email delivery is restricted to project team addresses; configure custom SMTP before sending OTPs to customers.

This folder is still a frontend preview for commerce. Orders, ownership, payment status, support tickets, reviews, catalogue management and Bookivo Bits use browser-side preview data and need database/API policies before live sales. The checkout layout is ready to receive the real Cashfree session flow, but it does not process a real payment yet.

Seeded customer reviews and placeholder catalogue titles have been removed. The review form requires an owned purchase and starts empty until verified reviews are returned by the server. The twelve visible catalog entries currently duplicate the Bachelor 99 Survival Cookbook as editable starting points. Road to 5Cr remains available through its existing direct landing page but is excluded from the catalog.

The About author card expects `assets/author.png`. The calculator CTA requires sign-in and then opens `Free calculater/index.html`; that page is expected to be added by the site owner. The Terms & Conditions, Privacy Policy and Refund & Cancellation Policy pages use the supplied policy draft. Confirm the owner, Grievance Officer and support contact details, and obtain legal review before publication.

## Pricing checks

- Browser: open `tests/pricing.test.html`.
- Node, when installed: run `node tests/pricing.test.js`.

Book cards and the detail page only show an MRP markdown badge when the computed markdown is at most 20%. The catalog retains the specified MRP values; this follows the prompt's stricter display cap when an MRP would imply a larger markdown.
