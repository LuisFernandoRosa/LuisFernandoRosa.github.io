# The LR Mark — Design

Personal monogram and site icon for Luis Fernando da Rosa.
Status: direction locked, ready to build.

---

## 1. Thesis

A struck plate. Two initials on the site's graphite ground, inside a ring
that catches the accent at one corner and fades to hair — the edge-rule
logic bent into a frame.

The mark is a **cipher-plate**: a monogram set inside a bordered field, the
way a bookplate or a silversmith's hallmark is. It is not a crest. Bevel's
source is precision metalwork, not heraldry, and a laurel-and-scroll
monogram would read expensive and still be wrong.

### Vocabulary

The register this site works in, named precisely, because the words drive
the decisions:

- **Quiet luxury** (*stealth wealth*) — value signalled through material and
  restraint, never through logos or ornament.
- **Editorial modernism** — magazine hierarchy, generous space, one accent.
- **Material honesty** — the palette is oxidised metal because it is meant
  to be metal.

"Old money" in its classic sense is *heraldic* — crests, laurels, engraved
scripts, a signet ring. This site is **bench luxury**, not **heirloom
luxury**. The mark follows the site.

---

## 2. Geometry

All values on a 100×100 viewBox. One file scales to every size.

| Element | Value |
|---|---|
| Tile | `100 × 100`, `rx 16`, fill `--void` `#151517` |
| Ring | inset `7`, `86 × 86`, `rx 10`, `stroke-width 2`, no fill |
| Ring stroke | linear gradient, `x1 0 y1 0 → x2 1 y2 1`: `--accent` at 0, `--hair` at 0.38, `--hair` at 1 |
| Letters | fill `--bone` `#E8E5DE`, no stroke |
| Letter box | `x 20 → 80` (60% width), `y 33.2 → 66.8` (cap height 33.6) |
| Tracking | `0.98 ×` the L's advance width |

The letters are centred on the tile, cap-block optically centred rather
than baseline-centred.

### Why radius 16

Radius 10 reads as a plaque, radius 24 as an app icon. 16 is rounded
enough to be soft at 16px and restrained enough not to look like a
launcher tile.

### Why ring weight 2.0

At 1.5 the ring ghosts even at 32px — it renders as a haze rather than a
line. 2.0 is the first weight that resolves. Above 2.5 it stops reading as
a hairline and becomes a border.

---

## 3. Letterforms

**Gelasio Regular**, converted to outlines. Two glyphs: `L` and `R`.

No emboldening. The outlines are the font exactly as drawn — no stroke, no
small-size cut. Stroking to add weight thickens hairlines proportionally
more than stems, which flattens the thick/thin contrast that gives a serif
its character. Fidelity wins over robustness here because the sizes where
the cut would help are sizes where the mark fails anyway (§5).

### Why the letters must be outlines

A favicon cannot load a webfont. `<text>` would render in whatever serif
the viewer's machine happens to have — different on every machine, and
absent on machines with no serif at all. This is true of any format, so it
is not a consequence of choosing SVG.

### Why Gelasio

Gelasio is metrically compatible with Georgia, and Georgia's shapes are
what read best in the early previews. Georgia itself is a licensed
Microsoft font, so shipping its outlines on a public site is not clean;
Gelasio is SIL Open Font License 1.1 and free to embed.

It is the sturdiest of the eight faces trialled, holding one size smaller
than any alternative. The trade is character: Georgia was engineered in
1993 for low-resolution screens, and it reads as a capable screen serif
rather than a heritage one. EB Garamond was the stronger fit for the
stated brief and was rejected in favour of robustness.

**Licence note:** Gelasio is OFL 1.1. Embedding glyph outlines in a logo is
permitted. Record the attribution in the repo; no on-page credit required.

---

## 4. Which letters

**LR** — Luis Rosa. Two glyphs is the ceiling for a mark that has to work
in a tab; LFR gives each letter about five pixels at favicon size. Dropping
the particle "da" is standard practice for Portuguese surnames in a mark.

---

## 5. Size behaviour

Measured by rasterizing the real SVG and inspecting pixels, not by
resizing mockups.

| Size | Result |
|---|---|
| 180 / 64 / 48 px | Crisp. Serifs and ring fully resolved. |
| 32 px | Reads cleanly. **This is what a Retina display shows in a 16px tab slot.** |
| 24 px | Marginal. Letters legible, serifs gone. |
| 16 px | Fails. Letters become grey mush, ring becomes haze. |

The 16px failure is accepted. It is inherent to putting three levels of
detail — ring, ground, letters — in 256 pixels, and it only affects 1×
displays. On any Retina screen the browser rasterizes the 16px tab slot at
32 device pixels, which is the row above.

---

## 6. Deliverables

| File | Purpose |
|---|---|
| `assets/img/logo.svg` | The master mark. 100×100 viewBox, ~3 KB, self-contained. |
| `assets/img/icon-32.png` | Fallback for browsers without SVG favicon support. |
| `assets/img/apple-touch-icon.png` | 180×180, iOS home screen and Safari. |

Head links added to `index.html`, `cv.html`, and `docs/post-template.html`
so future posts inherit them.

No build step. The PNGs are generated once and committed, consistent with
the site having no toolchain.

---

## 7. Placement

**Favicon only, for now.** The mark does not go on the page.

The masthead already carries two strong elements — the bevel and the
namemark — and rule 1 says the bevel is the only bold thing. A third mark
in the header competes with both.

This also settles an accent-discipline question. The design system allows
`--accent` in exactly five places and calls a sixth a regression. A favicon
lives outside the page, so the ring's accent does not count against that
budget. **The moment the mark appears on the page — footer included — it
becomes a sixth on-page accent use** and the ring must be reconsidered
(flat `--hair`, most likely). Deferred until there is a reason to place it.

---

## 8. Rejected

| Direction | Why not |
|---|---|
| Bevel bar in the tile | The bar is sub-pixel at 16px, so the concept vanishes at exactly the size it needs to work. Off-centres the letters at larger sizes. |
| Knocked-out letters on a filled accent tile | Best small-size survival of anything trialled, and the loudest — a solid accent field is a second bold element. |
| Gradient through the letters | Keeps the bevel logic without a bar, but puts accent into the letterforms, which forecloses ever placing the mark on the page. |
| Interlocked cipher (shared stem) | The traditional engraved monogram. Beautiful large, illegible small. |
| Hand-drawn Didone letterforms | Austere and generic next to a real text face. |
| Round-joined emboldening | Rounds every corner and blunts every serif tip. A defect, not a treatment. |

---

## 9. Quality floor

- Renders identically on light and dark browser chrome — the tile carries
  its own ground.
- No JavaScript, no external references, no embedded raster. Self-contained.
- Colours are literal hex, not CSS variables: an SVG loaded as a favicon has
  no access to the page's `:root`.
- If a token in `bevel.css` changes, the mark must be updated by hand. This
  is the one place in the project where a colour is duplicated, and it is
  unavoidable.

---

## 10. Open

- Whether the mark ever earns a place on the page, and the ring treatment
  if it does (§7).
- Whether a wordmark lockup — mark plus "Luis Fernando da Rosa" — is ever
  needed. Not needed for a favicon.
