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
- Validate markup: `python3 -c` sweep with `html.parser`, or open the file in a browser.
- Preview: `python3 -m http.server 12000` from the repo root.
  - Public URL: https://work-1-fdrygniyensbpkno.prod-runtime.all-hands.dev/ (port 12000)
  - A second host is available on port 12001: https://work-2-fdrygniyenspbkno.prod-runtime.all-hands.dev/

## Conventions
- Keep everything in `index.html`; do not split into separate CSS/JS files unless asked.
- `.font-heading` / `font-heading` class is used for headings instead of raw font-family.
- In-page anchors must have a matching `id` on the target section.
- Sections use `scroll-margin-top: 5rem` so the sticky header does not cover headings.
- Escape `&` as `&amp;` in text and attributes.

## Known behavior
- `#tourForm` submit is client-side only: it validates with `checkValidity()`, shows the message in `#formStatus`, and resets. There is no backend.
- `#videoModal` starts with `src=""` and gets the YouTube URL on open, then clears it on close, so the video stops playing.