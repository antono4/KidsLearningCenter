# AGENTS.md

## Project
Static single-page marketing site for **Little Stars Kids Learning Center**.

- `index.html` — the entire site (no build step, no dependencies installed locally).

## Stack
- Tailwind CSS via CDN (`cdn.tailwindcss.com`) with an inline `tailwind.config` block.
- Fonts: Fredoka (headings) + Quicksand (body) from Google Fonts.
- Icons: Font Awesome 6 free via CDN.
- Images: remote Unsplash URLs.
- Vanilla JS only, in a single `<script>` at the end of the file.

## Commands
- Validate markup: `python3` sweep with `html.parser` (checks tag nesting, duplicate ids, broken `#` anchors).
- Behaviour tests: append a `<script>` harness that calls the page's own functions and writes results to a
  `<pre id="TESTOUT">`, then run
  `chromium --headless --no-sandbox --disable-gpu --disable-dev-shm-usage --virtual-time-budget=9000 --dump-dom file:///tmp/test.html`
  and parse the `TESTOUT` block. This catches logic bugs that markup checks miss.
- Contrast: compute WCAG ratios in Python. Minimum 4.5:1 for normal text, 3:1 for large/bold text.
- Preview: `python3 -m http.server 12000` from the repo root.
  - Public URL: https://work-1-fdrygniyensbpkno.prod-runtime.all-hands.dev/ (port 12000)
  - A second host is available on port 12001: https://work-2-fdrygniyensbpkno.prod-runtime.all-hands.dev/
- Note: `--window-size` in headless Chromium does not change `clientWidth` (reports ~485 at 390px).
  Use the real browser tool for viewport-specific checks.

## Conventions
- Keep everything in `index.html`; do not split into separate CSS/JS files unless asked.
- `.font-heading` / `font-heading` class is used for headings instead of raw font-family.
- In-page anchors must have a matching `id` on the target section.
- Sections use `scroll-margin-top: 5.5rem` so the sticky header does not cover headings.
- Escape `&` as `&amp;` in text and attributes.
- Use `motion-safe:` for transforms/animations so reduced-motion users are not affected.
- Icons that are decorative get `aria-hidden="true"`; interactive icon-only buttons get `aria-label`.
- Images carry `alt`, `width`/`height`, `decoding="async"`, `loading="lazy"`, and class `photo`
  (a slate placeholder tint that shows before load).

## Colour rules (WCAG AA, verified by computation)
Do not use these on white - they fail AA for normal text:
- `text-amber-500` (2.15:1) -> use `text-amber-700` (5.02:1)
- `text-pink-500` (3.53:1) -> use `text-pink-700`
- `text-sky-600` (4.10:1) -> use `text-sky-700` (5.93:1)
- `text-emerald-600` (3.77:1) -> use `text-emerald-700`

On tinted surfaces:
- Footer is `bg-sky-800`; `text-sky-100` body (6.59:1) and `text-amber-300` headings (5.25:1) pass.
  At `bg-sky-600` both failed (3.57:1 and 2.84:1).
- Activities section uses `.bg-sky-pattern`; `text-sky-100` body passes at 5.17:1.
- Pills: dark-800 text on light-100 chip (6.3-6.8:1). Do not pair `-700` on `-200` (4.03:1).
- Form labels are `text-slate-900` on `bg-amber-400` (10.69:1); errors `text-red-800` (4.98:1).
- Inputs use `placeholder-slate-500` (4.76:1), not `placeholder-slate-400` (2.56:1).

## Known behavior
- `#tourForm` submit is client-side only: per-field validation writes messages into
  `#tourNameError` / `#tourEmailError` / `#tourPhoneError`, sets `aria-invalid` and a red border, and
  `#formStatus` shows the success text. `hideStatus()` runs on every submit so a stale "thanks" message
  never lingers next to fresh validation errors. There is no backend.
- `#videoModal` starts with `src=""` and gets the YouTube URL on open, then clears it on close, so the
  video stops playing. Escape and backdrop clicks close it; focus is restored to the trigger, and Tab is
  trapped inside while open.
- `.nav-link` elements get `aria-current="true"` plus `text-sky-700` / `bg-sky-50` from an
  IntersectionObserver scroll-spy. This works in a real browser; in headless Chromium the observer can
  report the first section because the synthetic viewport does not scroll.