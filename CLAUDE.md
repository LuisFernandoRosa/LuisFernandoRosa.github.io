# CLAUDE.md

Personal website + blog for Luis Fernando da Rosa. Direction: **Bevel** —
graphite ground, one chrysocolla accent, bled portrait, ground-edge signature.

## The three rules (never violate)

1. **One bold element.** The bevel is it. Everything else stays quiet.
2. **Fade the ground, never the subject.** Masks/gradients dissolve background only.
3. **It has to survive a bad post.** This exists to be written in, not admired.

## Where to change things

- **Accent color, fonts, all design tokens →** `assets/css/bevel.css` `:root`
  (top of the file). Change once, it propagates everywhere. New accent values
  must pass the shine rule: L ≥ 0.68, C ≥ 0.07.
  - Changing a **font** means updating the `:root` variable **and** the Google
    Fonts `<link>` in the `<head>` of each HTML page.
- **Site identity / nav (name, tagline, links) →** written in the HTML of
  `index.html` and `cv.html`. Keep them consistent by hand — there is no
  template engine, by design.
- **Analytics →** a GoatCounter pixel (`<img>`, no JS) at the bottom of each
  page's `<body>`. Dashboard: <https://luisfernandorosa.goatcounter.com> —
  private, and stays that way. Every page needs its **own** `?p=` matching its
  URL, or they all collapse into one row.

## Run it

No build step, no dependencies.

    python3 -m http.server 8000

…or just open `index.html`. **No client-side JavaScript — keep it that way.**
The analytics pixel is a plain `<img>` precisely so this rule survives; if you
ever swap it for GoatCounter's `count.js`, you are amending the rule, not
working around it — say so here.

## Add a blog post

Write the post in Markdown and hand it over. The Markdown is kept as the
source at `writing/<slug>.md` with front matter (`title`, `slug`, `date`,
`category`, `tags`, `dek`, `reading_time`), and rendered by hand into a
standalone Bevel HTML page at `writing/<slug>.html` following
`docs/post-template.html`, then linked from the homepage index (`.entry`
row: date + category on the left, title + dek on the right). Code blocks are `--bone` on `--surface`
with no syntax colouring; diagrams are inline SVG (no JS).

**Don't forget:** replace `{{ SLUG }}` in the analytics pixel at the bottom of
the template with the post's filename. Miss it and the post's views land on a
literal `/writing/{{ SLUG }}` row in the dashboard.

## Deploy

GitHub Pages serves the repo directly (`.nojekyll`). Target repo:
`LuisFernandoRosa.github.io` (root URL). Push to the default branch = live.

## Source of truth

`docs/design-system.md` — tokens, rules, components, constraints. Read it
before changing anything visual; the values that look arbitrary aren't.
