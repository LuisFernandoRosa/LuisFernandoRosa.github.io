# Bevel — Design System

Personal blog / writing site for Luis Fernando da Rosa.
Status: direction locked, ready to build.

---

## 1. Thesis

Modern restraint with material warmth. Cold graphite ground, one metal
accent, generous space, and a single structural signature that carries
the whole identity.

The design comes from precision metalwork, not from other blogs. That is
the source of everything below — the palette is oxidised metal, the
signature is a ground edge, the vocabulary is bench vocabulary.

**Three rules that govern every decision:**

1. **One bold element.** The bevel is it. Everything else stays quiet.
   The moment a second element competes — a bright photo, a second
   accent, a large graphic — the signature stops reading and the page
   becomes generic dark-mode.
2. **Fade the ground, never the subject.** Applies to the portrait mask,
   to gradients, to any future imagery. Dissolving a face or a word looks
   like a bug, not a choice.
3. **It has to survive a bad post.** This site exists to be written in,
   not admired. Any addition that raises the bar for what deserves to be
   published is a regression.

---

## 2. Colour

### Tokens

| Token | Hex | OKLCH | Role |
|---|---|---|---|
| `--void` | `#151517` | `oklch(0.19 0.004 285)` | Page background. Everything fades into this. |
| `--surface` | `#1C1D20` | `oklch(0.23 0.005 285)` | Hover state on index rows. The only raised surface. |
| `--bone` | `#E8E5DE` | `oklch(0.92 0.008 85)` | Body text, headings. |
| `--muted` | `#8B8880` | `oklch(0.61 0.007 80)` | Metadata, deks, mono labels. |
| `--accent` | `#66BEAB` | `oklch(0.74 0.09 178)` | Chrysocolla. The bevel, the italic surname, rules, hover ticks. |
| `--accent-deep` | `#357669` | `oklch(0.52 0.07 178)` | Category labels only. Never for text that must be read. |
| `--hair` | `#2C2D31` | `oklch(0.27 0.006 285)` | All 1px rules and borders. |

### The shine rule

An accent reads as *metal* on a dark ground when lightness sits at or
above **L 0.68** with chroma at or above **C 0.07**. Below that it reads
as paint.

This is why `--accent-deep` (L 0.52) is correct for recessive labels and
wrong for anything else, and it's the test to apply to any future colour.
Hue is free; L and C are not.

### Accent discipline

The accent appears in exactly five places:

- the bevel
- the italic surname in the masthead
- the left edge of a pull quote
- the hover tick on an index row
- the underline of an inline text link, **on hover only** (rest state is
  `--hair`)

Plus `--hair`-weight rules that fade from accent to hair. A sixth use is
a regression. Links in body copy are `--bone` with a `--hair` underline at
rest; the underline shifts to accent on hover/focus as a structural
"cutting edge" cue, consistent with the bevel and the edge-rule gradient.
Accent never becomes the link's text color and never appears at rest —
only on the hover/focus transition.

### Cyan guard

At hue 178° the accent is close enough to cyan that a second cool element
tips the page into "terminal." Code blocks are `--bone` on `--surface`
with **no syntax colouring**, or warm greys only. The accent stays the
only chromatic thing on the page.

---

## 3. Typography

| Role | Face | Weights | Notes |
|---|---|---|---|
| Display | Instrument Serif | 400, 400 italic | Name, post titles, pull quotes. Never below ~1.5rem — it falls apart small. |
| Body | Archivo | 300, 400 | Prose and deks. **300 is the default.** The page reads light. |
| Utility | JetBrains Mono | 400 | Dates, categories, footer. |

### Scale

```
namemark      clamp(3.2rem, 9.5vw, 6.6rem)   display 400   lh 1.00   ls -0.012em
post title    clamp(2.4rem, 6.4vw, 4.2rem)   display 400   lh 1.03
entry title   clamp(1.55rem, 3.6vw, 2.4rem)  display 400   lh 1.13   max 24ch
pull quote    1.85rem                        display 400   lh 1.28   max 26ch
standfirst    1.14rem                        body 300      lh 1.55   max 32ch
prose         1.08rem                        body 300      lh 1.70   max 60ch
dek           1.00rem                        body 300      lh 1.55   max 54ch
label         10.5px                         mono 400      ls 0.18em  uppercase
```

Measure is capped everywhere. Prose at 60ch, titles at 24ch — long
headlines wrap to two lines by design rather than running the full column.

### Utility labels

Always uppercase, always `letter-spacing: 0.18em`, always `--muted`.
They're instrument markings, not text. Never sentence case, never larger
than 10.5px.

---

## 4. The signature: the bevel

```css
.bevel{
  position:fixed; left:0; top:0; bottom:0; width:3px; z-index:5;
  background:linear-gradient(to bottom,
    rgba(0,0,0,0) 0%,
    color-mix(in srgb, var(--accent) 18%, transparent) 18%,
    color-mix(in srgb, var(--accent) 85%, transparent) 62%,
    rgba(232,229,222,.95) 100%);
}
```

A ground edge: dull at the spine, bright at the cutting edge. Fixed to
the viewport, so it stays present through the whole scroll — the one
constant on every page.

Do not animate it, do not thicken it, do not add a second one on the
right. It works because it is the only thing of its kind.

---

## 5. Portrait

Bled from the right. 56% viewport width, `min(880px, 102vh)` tall, masked
to transparent leftward and faded to `--void` at top and bottom. No
border, no frame, no radius.

```css
mask-image:linear-gradient(to left,
  #000 0%, #000 42%,
  rgba(0,0,0,.62) 68%,
  rgba(0,0,0,.16) 87%,
  transparent 100%);
filter:saturate(.42) contrast(1.04) brightness(.86);
background-position:58% 26%;
```

The solid zone extends to 42% so the face never falls below full opacity.
This is the rule-2 case: the mask dissolves the background only.

### Requirements for any replacement photo

- **Dark left side.** The mask eats that edge; if it's lit, the fade looks broken.
- Charcoal seamless background falling to near-black at the frame edges.
- Single soft key from one side, ~4:1 ratio. Deep retained shadow.
- Vertical 3:4, headroom above the head — the layout crops the top.
- Plain dark garment, no logos. Neutral expression.
- Desaturate to ~55%, warmth retained only in skin highlights.

### Mobile

Moves to a top band at 100% width / 62vh, drops to `brightness(.42)`,
and fades **downward** instead of leftward.

---

## 6. Layout

```
content column   max-width 960px, padding 0 40px (26px mobile)
masthead         padding-top 20vh desktop / 26vh mobile
index row        grid 130px + 1fr, gap 2.4rem
                 collapses to single column under 680px
prose column     max-width 60ch
```

Vertical rhythm runs on viewport units at the section level (`10vh`,
`12vh`) and rems within sections. Sections breathe; content doesn't.

---

## 7. Components

### Index row

```
[ date        ]  Post title in display serif
[ category    ]  Dek in body 300, muted, max 54ch
```

- Date in `--muted`, category in `--accent-deep`
- Hover: background to `--surface`, plus a 1px accent tick growing to 64%
  height at the left edge
- Transition `.45s cubic-bezier(.2,.8,.3,1)` on both
- Bottom border `1px solid var(--hair)`

The tick is the bevel's echo — same idea at row scale. It's the only
place that echo is allowed.

### Pull quote

Display serif, 1.85rem, `border-left: 1px solid var(--accent)`, 1.8rem
left padding, max 26ch. No quotation marks, no attribution styling, no
centring.

### Edge rule

```css
background:linear-gradient(to right, var(--accent), var(--hair) 34%, var(--hair));
```

Section dividers pick up the accent at their left edge and fade to hair.
Directional, like everything else on the page.

---

## 8. Motion

- Single easing curve: `cubic-bezier(.2,.8,.3,1)`
- Single duration: `.45s`
- Only two animated properties: `background` on rows, `height` on ticks

No scroll animation. No page transitions. No parallax on the portrait —
it is fixed material, not a moving layer.

```css
@media (prefers-reduced-motion:reduce){
  *{transition:none!important; animation:none!important}
}
```

---

## 9. Quality floor

- Responsive from 320px up. Single breakpoint at 680px, one at 820px.
- Visible keyboard focus: `1px solid var(--accent)`, `offset 6px`
- Reduced motion respected
- Body text `--bone` on `--void` — contrast ratio ~14:1
- `--muted` on `--void` — ~5.4:1, passes AA for its sizes
- **Never** use `--accent-deep` for body text; it fails contrast and is
  scoped to labels for that reason
- Semantic HTML. The index is `<main>`, entries are `<a>` wrapping
  headings, the portrait is `aria-hidden` decoration

---

## 10. Build notes

Self-host the three faces before launch. Three Google Fonts requests is
the slowest thing on an otherwise near-instant page. Subset to Latin +
Latin Extended-A (Portuguese needs ã, õ, ç, á, é, í, ó, ú, â, ê, ô).

The portrait is currently base64-embedded so the prototype is a single
portable file. In the real build it becomes a normal asset — serve AVIF
with JPEG fallback, ~900px wide is enough at the opacity it runs at.

Static site generator: anything that outputs plain HTML. The design has
no JavaScript at all in its final form, and it should stay that way.

---

## 11. Open decisions

- Real photograph to replace the generated placeholder
- Post page layout beyond the reading view (footnotes, code blocks, images in prose)
- Whether the site carries a name or stays under the personal name
- RSS, and whether the footer links are the full navigation or a nav bar is needed once there are more than ~15 posts
