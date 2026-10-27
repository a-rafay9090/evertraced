# Evertraced — v2 (matches the navy/teal/pink/yellow mock)

A fully responsive, multi-page rebuild of the second design, with every nav link, button, and CTA wired to a real page.

## Pages
- `index.html` — Home: hero window, memories band, three-steps/parachute section, four editions, ocean/ship section, how-it-works, pricing, sign-up teaser.
- `signin.html` — Dedicated sign-in page.
- `signup.html` — Dedicated create-account page (first/last name, email, password + social buttons).
- `account.html` — Signed-in account dashboard: profile card (avatar, name, email, member-since date) with an inline edit form, stat tiles (books started / photos added / orders placed), a list of your drafted books linking to their previews, order history from any completed checkouts, and account actions (sign out, delete account). Redirects to `signin.html` if no one is signed in.
- `studio.html?type=travel|memoir|legacy|wedding` — "Create Book" wizard: template → drag-and-drop photo upload (in-browser previews) → captions → live cover preview + compose animation.
- `package.html?plan=digital|hardcover` — Package selection and a demo checkout (shipping fields appear only for the hardcover plan) ending in an order confirmation.
- `ebooks.html` — Filterable gallery of example books with a sample-preview modal.
- `preview.html?type=travel|memoir|legacy|wedding` — Flip through the book you composed in the studio: a cover page followed by one page per uploaded photo and caption, with prev/next buttons, page dots, and arrow-key navigation. If that edition hasn't been composed yet, it shows an empty state with a switcher to any editions you *have* drafted and a link back to the studio.

## Fixes from the pasted source
- Removed all now-orphaned/duplicated code: the commented-out old `<nav>`, the malformed `background: rgba(...);[]` rule, the stray backtick in `scale(1.05\`)`, and the mismatched `</section>` nesting around `#how`/`#ocean`/`#pricing`/`#signup`.
- Replaced hardcoded local image paths (`cover.png`, `sky.png`, `1000078526.gif`, `ts.gif`, a `file:///C:/...` video, etc.) that only existed on the original author's machine with CSS/SVG illustration in the same palette, so the site works anywhere.
- Nav links, "Begin Now"/"Get Yours Now"/"Choose My Book" buttons, and the four edition cards now go to real, working destinations instead of `href="#"` or nothing.

## How to run it
No build step — open `index.html` directly, or serve the folder locally:
```
python3 -m http.server 8000
```
then visit `http://localhost:8000`.

## What's real vs. simulated
Front-end only, no server:
- Accounts and studio drafts (including the uploaded photos, as data URLs) save to the browser's `localStorage`, which is what powers the preview page.
- Uploaded photos are read with `FileReader` and never leave the device.
- "Compose Preview" and checkout are simulated with progress/confirmation UI rather than a real render or payment.
- Social sign-in buttons show a toast instead of real OAuth.

## Responsive behaviour
Single shared stylesheet (`css/styles.css`) with a mobile nav (hamburger + slide-down menu) under 860px, stacking grids (cards, pricing, plans, forms) under 760px, and a tuned hero window at 480px and below.

## Scroll animations
Reveal-on-scroll is powered by GSAP + the free **ScrollTrigger** plugin (loaded from cdnjs), in `js/scroll-animations.js`:
- `.reveal` elements fade up as they enter the viewport; `.reveal-left` / `.reveal-right` slide in from the side.
- Book cards on `mybooks.html` fade + scale in with a staggered delay.
- Category/pricing/plan cards get a quick staggered pop.
- All of it respects `prefers-reduced-motion` (skips straight to the visible state) and reverses gracefully if you scroll back up.

Note: **ScrollSmoother** (buttery inertia-smoothed scrolling) is a paid Club GreenSock plugin and isn't included here — only the free ScrollTrigger is used. If `js/main.js` ever runs before GSAP loads (e.g. a CDN hiccup), it falls back to a plain `IntersectionObserver` fade so content never gets stuck hidden.
