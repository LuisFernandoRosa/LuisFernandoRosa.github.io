# personal-blog

Static prototype of the blog. Direction: **Bevel** — graphite ground,
chrysocolla accent, bled portrait, ground-edge signature.

```
index.html                 the page
assets/css/bevel.css       all styles, tokens at the top in :root
assets/img/portrait.jpg    header portrait (placeholder — generated)
docs/design-system.md      the spec: tokens, rules, components, constraints
```

## Run it

No build step. Open `index.html`, or:

```bash
python3 -m http.server 8000
```

There is no JavaScript. Keep it that way.

## Before you change anything

Read `docs/design-system.md` first — specifically the three rules in §1
and the accent discipline in §2. Most of the values in the CSS look
arbitrary and aren't; the doc explains which ones are load-bearing.

The short version:

- **One bold element.** The bevel is it.
- **Fade the ground, never the subject.**
- **It has to survive a bad post.**

## Known TODO

- Replace `assets/img/portrait.jpg` with a real photograph.
  Requirements are in §5 of the design doc — the dark left side is not
  optional, the mask depends on it.
- Self-host Instrument Serif, Archivo, and JetBrains Mono. Currently
  loaded from Google Fonts, which is the slowest thing on the page.
  Subset to Latin + Latin Extended-A for Portuguese diacritics.
- Post template. `index.html` includes a reading-view sample at the
  bottom; that's the type treatment, not a real template yet.
- Decide on the static site generator. Anything that emits plain HTML.
