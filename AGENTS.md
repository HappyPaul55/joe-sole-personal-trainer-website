# AGENTS.md

## Project overview

Personal trainer website for **Joe Sole** — a single-page marketing site with a dark,
neon-gym aesthetic. Single-file implementation: `index.html` with embedded CSS and
vanilla JS. **No frameworks, no build step, no dependencies** — only external resource
is Google Fonts (Anton + Space Grotesk). Open `index.html` directly in a browser to run.

## Files

| File | Purpose |
|------|---------|
| `index.html` | Entire site: HTML, CSS (`<style>`), JS (`<script>`) |
| `me.jpg` | Joe's photo, used 3× (hero, about, quote banner). Currently shows Rafal's photo — replace with Joe's (same filename or update the 3 `src` refs) |

## Design system

Defined as CSS custom properties in `:root`:

- `--bg: #0b0b0d`, `--surface: #131316`, `--line` (subtle borders)
- `--accent: #c6ff00` (lime), `--accent-2: #ff5c00` (orange), `--text`, `--muted`
- Display font: Anton (`--display`), body: Space Grotesk (`--body`)
- Recurring motifs: angled `clip-path` shapes, outlined text (`-webkit-text-stroke` + transparent color), `backdrop-filter` blur, film-grain `.noise` overlay, scroll progress bar, `prefers-reduced-motion` support (disables animation, forces all reveals/bars visible)

## Page structure (top to bottom)

1. **Nav** — fixed, blurs on scroll (`.scrolled`), burger menu < 700px. Links: About, Results, Programs, Method, Contact
2. **Hero** — full-viewport (100svh), background grid + glows, giant headline (solid + outline spans), photo in angled frame with floating PR chips and spinning SVG badge, scroll hint
3. **Marquee** — tilted infinite scroll strip, pauses on hover, content duplicated ×2 in HTML for the loop
4. **About** — bio paragraphs, 4 fact cards, angled photo with "CERTIFIED PT" badge (CSS `content`)
5. **Results (`#prs`)** — 4 stat counters with count-up animation
6. **Programs (`#training`)** — 3 cards (1-on-1 / Online / Small Group) + rest note
7. **Method (`#goals`)** — 4 animated progress bars
8. **Contact (`#contact`)** — CTA band with `mailto:` booking buttons
9. **Quote** — full-width photo banner with overlay + blockquote
10. **Footer** — giant outlined wordmark, social placeholder links, auto-updating year

## JS behavior (single IIFE, ~3KB)

- Scroll progress bar width + nav `.scrolled` toggle (rAF-throttled)
- Mobile menu toggle (`#burger` / `#navLinks`)
- IntersectionObserver scroll reveals (`.reveal` → `.visible`, `data-delay` 1–3)
- Count-up animation for `[data-count]` elements when visible (ease-out cubic)
- Progress bars: JS sets `--w` from `data-width`, observer adds `.go` → CSS transitions width

## Customization points

- **Photo:** replace `me.jpg` (3 references: hero img, about img, quote banner bg)
- **Email:** `mailto:hello@joesole.coach` (2 links in `#contact`), plus nav CTA text
- **Stats:** `data-count` values in `#prs`; labels in `.label` divs
- **Program mix:** `data-width` values in `#goals`
- **Socials:** `href="#"` placeholders in footer
- Top-of-file HTML comment summarizes these

## Conventions / gotchas

- No emojis in copy; separators use `&#10022;` (✢)
- Anchor ids (`#about`, `#prs`, `#training`, `#goals`, `#contact`) must match section ids — nav links and hero/nav CTAs point at them
- `scroll-padding-top: 88px` on `html` compensates for fixed nav
- Keep `-webkit-` prefixes alongside standard properties (mask, backdrop-filter, text-stroke)
- Marquee loop requires the content block duplicated exactly ×2 for seamless `translateX(-50%)`
- Never use frameworks here — the constraint is deliberate

## Verification

No build or test suite. Check: inline `<script>` parses, tag balance, anchor targets exist. Visual check requires a browser — none available in this environment's tooling (browser binary absent), so static checks are the norm.
