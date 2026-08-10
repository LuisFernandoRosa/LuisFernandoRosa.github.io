# Personal Website — Phase 1 Design Spec

**Foundation + CV page**
Date: 2026-08-10
Status: approved in brainstorming, ready for implementation planning
Owner: Luis Fernando da Rosa

---

## 1. Goal & scope

Turn the existing "Bevel" static prototype into a real, maintainable personal
website with a proper foundation and a curriculum (CV) page. This is **Phase 1**.

**In scope (Phase 1):**

- Git repository with a clean, logical commit history
- Project structure and conventions
- Single-source configuration for design tokens (fonts, accent)
- `CLAUDE.md` (kept simple) and updated `README.md` / instructions
- A `/cv` curriculum page rendered in the Bevel style
- Homepage cleaned up (no fake content), ready to receive real posts later
- Deployment target decided and documented (not necessarily pushed in P1)

**Out of scope (later phases):**

- The blog authoring pipeline and first real posts (Phase 2)
- Self-hosting the three typefaces (currently Google Fonts)
- RSS, richer post features (footnotes, code blocks in prose, images), custom domain

---

## 2. Architecture decision

**Pure static HTML + CSS. No static-site generator, no build step, zero
dependencies.**

Rationale — the user's explicit priority is *"the simplest setup that works
without problems and constant fixing."* Every build tool (Hugo, Eleventy, Astro)
introduces a toolchain that can break on upgrades. A plain static site has no
toolchain to break.

### Blog authoring workflow (the "build step" is Claude)

1. User writes a post in Markdown and hands it over.
2. Claude renders it into a Bevel-styled standalone HTML page following
   `docs/post-template.html`, adds it to the homepage index, and commits.
3. Nothing runs on the user's machine. Nothing to update, nothing to break.

### Hosting

- **GitHub Pages**, serving the repository directly.
- `.nojekyll` file disables Jekyll processing — the repo is served as-is.
- Push to the default branch = live. No GitHub Action required.

### JavaScript

- **No client-side JS by default** — the Bevel design depends on this
  (design-system.md §8, §2 "cyan guard").
- Because the site is plain HTML, a *small, deliberate* piece of JS can be added
  later for a specific interactive artifact if ever needed — no framework, so the
  door stays open at zero standing cost. Any such addition is a conscious
  exception, not the norm.

### Diagrams / artifacts (future)

Committed as pre-rendered **SVG** embedded directly in the post HTML. Stays pure
static; honors the "one bold element" and "no second chromatic element" rules.

---

## 3. Project structure

```
personal-blog/
  index.html                 homepage: masthead + Writing index (empty state for now)
  cv.html                    curriculum page
  writing/                   blog posts, one standalone .html each (empty for now)
  assets/
    css/bevel.css            all design tokens in :root — SINGLE PLACE for fonts + accent
    img/portrait.jpg         header portrait
    Luis-Fernando-Rosa-Resume.pdf   downloadable résumé (copied into repo)
  docs/
    design-system.md         unchanged — the design source of truth
    post-template.html       the skeleton Claude fills in per post (from the reading-view sample)
    superpowers/specs/       design specs (this file)
  CLAUDE.md                  project instructions (simple)
  README.md                  how to run / how to add a post / where to change things
  .nojekyll                  tells GitHub Pages to serve the repo as-is
  .gitignore                 OS/editor cruft
```

---

## 4. Single-source configuration

Two obvious, documented places to change things:

1. **Design tokens (accent color, fonts, all colors) →** `assets/css/bevel.css`
   `:root`.
   - Change `--accent` once → propagates to the bevel, italic surname, rules, hover
     ticks (per design-system.md §2). Any new accent value must pass the "shine
     rule": L ≥ 0.68, C ≥ 0.07.
   - Change `--display` / `--body` / `--util` once for fonts. **Note:** the font
     *files* are also loaded via a `<link>` to Google Fonts in each page `<head>`;
     changing a typeface means updating both the variable and that link. `CLAUDE.md`
     documents this pair explicitly so it is not a hunt.

2. **Site identity / nav (name, tagline, email, GitHub/LinkedIn links) →** written
   directly in the HTML. With no template engine there is minor duplication across
   the two pages; it is small and Claude keeps it consistent. A templating
   dependency is deliberately **not** added just to remove a few duplicated lines —
   that would reintroduce the maintenance cost the whole architecture avoids.

---

## 5. Pages

### 5.1 Homepage (`index.html`)

- Keep the existing masthead (name, label, standfirst personality line, edge rule).
- Add a small, tasteful **nav** including a link to the CV.
- **Writing** section shows a quiet empty state ("First entries soon") instead of
  the current placeholder posts — no fabricated content on a live site.
- The current reading-view sample block is **moved out** into
  `docs/post-template.html` to serve as the per-post reference template.

### 5.2 CV page (`cv.html`)

Rendered in the Bevel style (not a copy of the résumé PDF). Wording is **rewritten
for the strongest impression** in the Bevel voice. Claude may leave weak-signal
items out and will **ask before adding** anything not present in the source
material.

**Sections, in order:**

1. **Masthead** — name; the positioning line
   (`AI/ML Engineer · Agentic Systems · Production AI from 0→1`); location (Paraná,
   Brazil); contact/social links (email, LinkedIn, GitHub).
2. **What I bring** — the summary paragraph as a standfirst.
3. **Experience** — the through-line of the page. All roles below, each as
   `role · company · dates` plus tight, rewritten bullets. Recent/high-impact roles
   get 2–3 bullets; older roles get 1–2. Ordering is reverse-chronological:
   - AI Engineer · Golabs Tech (Jun 2026 – Present) — multi-agent engineering system, eval harness, tracing, enterprise-hosted models
   - AI Engineer · Arionkoder (Jun 2025 – May 2026) — bot_id HTML injection technique, 4-stage extraction pipeline, LLM-as-judge
   - Senior AI Engineer · Flow+ (Nov 2023 – May 2025) — burnout detection product, end-to-end AI delivery ownership
   - Freelance AI Engineer (Oct 2024 – Jan 2025) — RAG over financial/inventory corpora
   - Computer Vision Engineer · Pix Force (Feb 2023 – Dec 2023) — LiDAR coal-silo volume (<5% error), OCR+GPT doc analysis
   - Computer Vision Engineer · GETTER (May 2022 – Jan 2023) — edge/IoT vision at <200ms, Docker deployment pipelines
   - Computer Vision Lead & Researcher · LAMIA – UTFPR (Sep 2020 – Sep 2022) — led CV research team, bare-metal k8s, Wood-Inspector
   - Software Engineer · Silicon Village (Jul 2021 – Apr 2022) — event-driven serverless backend
4. **Technical toolkit** — grouped as quiet mono labels:
   Agents & LLMs / Agent reliability / ML & CV / Data & storage / Infrastructure.
5. **Notable project** — Wood-Inspector Amazônia (patent documented; link
   `clb.lamia.sh.utfpr.edu.br`).
6. **Download** — a "Download résumé (PDF)" link pointing at the copy in
   `assets/`.

**No education section** — per the user's decision, the page focuses on experience.

---

## 6. Design compliance

The CV and homepage obey the existing design system (docs/design-system.md) with
no exceptions:

- **One bold element** — the bevel. No second accent, no bright graphic.
- **Fade the ground, never the subject** — the portrait mask rule stands.
- Accent used only in its sanctioned places; category/meta labels in `--muted` /
  `--accent-deep`, uppercase mono, ≤10.5px.
- Typography scale and measure caps from §3 apply to the CV as they do to posts.
- Quality floor from §9: responsive from 320px, visible keyboard focus, reduced
  motion respected, AA contrast, semantic HTML.

---

## 7. Git & commit history

- `git init` on `personal-blog/`.
- `.gitignore`: `.DS_Store`, editor/OS cruft. (No build artifacts to ignore.)
- `.nojekyll` committed so GitHub Pages serves the repo as-is.
- Clean, logical, self-contained commits — each builds/opens correctly. Proposed
  sequence (finalized in the implementation plan):
  1. `docs: add Phase 1 design spec` (this document) + `.gitignore`
  2. Project scaffold: `.nojekyll`, directory layout
  3. Design tokens / config pass on `bevel.css` (single-source cleanup + comments)
  4. Homepage: nav + Writing empty state; move sample → `docs/post-template.html`
  5. CV page (`cv.html`) + résumé asset
  6. `CLAUDE.md` + `README.md` / instructions
- Commit messages end with the required `Co-Authored-By` trailer.

---

## 8. Deployment (documented; push is the final step)

- **GitHub Pages**, repo named **`LuisFernandoRosa.github.io`** so it serves at the
  root URL (`https://luisfernandorosa.github.io`).
- Static repo + `.nojekyll`; serve from the default branch root. No Action.
- Final push via `gh` if installed, otherwise documented manual commands. Creating
  the remote/enabling Pages is done with the user, not silently.

---

## 9. CLAUDE.md contents (kept simple)

- One-line description of the project.
- The three Bevel rules (verbatim intent).
- **Where to change things:** the two single-source locations from §4.
- **How to run:** open `index.html`, or `python3 -m http.server 8000`.
- **How to add a post:** hand Claude the Markdown; it renders per
  `docs/post-template.html` and updates the index.
- **No client-side JS by default** — and why.
- Pointer to `docs/design-system.md` as the source of truth.

---

## 10. Success criteria

- Site opens with **no tooling** (double-click `index.html`); both pages render
  correctly per the design system.
- Design tokens are genuinely single-source: changing `--accent` or a font family
  updates the whole site from one place (font also requires the documented `<link>`
  update).
- CV makes a strong, honest impression; experience-led; no fabricated claims.
- Git history is clean and each commit is self-contained.
- `CLAUDE.md` answers "where do I change X?" and "how do I add a post?" without a
  hunt.
- Passes the design quality floor (contrast, responsive 320px+, focus visibility,
  reduced motion).
