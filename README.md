# personal-blog

Personal website + writing for Luis Fernando da Rosa. Direction: **Bevel** —
graphite ground, chrysocolla accent, bled portrait, ground-edge signature.
Pure static HTML + CSS, no build step, no JavaScript.

```
index.html                  homepage: masthead + Writing index
cv.html                     curriculum (experience-led)
writing/                    blog posts (one standalone .html each)
assets/css/bevel.css        all styles + design tokens in :root
assets/img/portrait.jpg     header portrait
assets/*.pdf                downloadable résumé
docs/design-system.md       the design spec (source of truth)
docs/post-template.html     per-post skeleton
CLAUDE.md                   working instructions
```

## Run it

No build step, no dependencies:

    python3 -m http.server 8000

…or open `index.html` directly.

## Changing the look

- **Accent / fonts / colours →** `assets/css/bevel.css` `:root` (one place).
  Changing a font also means updating the Google Fonts `<link>` in each page head.
- **Name / tagline / links →** the HTML of `index.html` and `cv.html`.

Read `docs/design-system.md` first — the three rules in §1 and accent discipline
in §2 are load-bearing.

## Deploy

GitHub Pages, served directly via `.nojekyll`. Push to the default branch of
`LuisFernandoRosa.github.io` and it's live at the root URL.
