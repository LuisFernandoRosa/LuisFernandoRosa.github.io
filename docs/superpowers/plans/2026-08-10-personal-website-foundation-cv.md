# Personal Website — Foundation + CV Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Turn the Bevel static prototype into a maintainable, git-tracked personal website with a foundation (config, docs, instructions) and an experience-led CV page, deployable to GitHub Pages.

**Architecture:** Pure static HTML + CSS, no build tool, no client-side JS. GitHub Pages serves the repo directly via `.nojekyll`. Blog posts (Phase 2) will be authored in Markdown and rendered to Bevel HTML by Claude following `docs/post-template.html`.

**Tech Stack:** HTML5, CSS3 (custom properties for design tokens), Google Fonts (Instrument Serif / Archivo / JetBrains Mono). No JS, no dependencies, no package manager.

## Global Constraints

- **No client-side JavaScript** by default (design-system.md §8). Any exception is deliberate and framework-free.
- **One bold element:** the bevel. No second accent, no bright graphic (design-system.md §1, §2).
- **Fade the ground, never the subject** (design-system.md §1 rule 2).
- **Accent (`--accent`) only** in: the bevel, the italic surname, pull-quote left edge, hover ticks, and accent→hair rules. A fifth use is a regression (design-system.md §2).
- **Design tokens live only in** `assets/css/bevel.css` `:root`. New accent values must pass the shine rule: L ≥ 0.68, C ≥ 0.07.
- **Site identity** (name, tagline, links) is written in HTML; keep it consistent across pages by hand (no template engine by design).
- **No fabricated CV claims.** Rewrite wording from the résumé/LinkedIn source only; ask the user before adding anything new.
- **Semantic HTML**, responsive from 320px, visible keyboard focus (`1px solid var(--accent)`, offset 6px), `prefers-reduced-motion` respected, AA contrast.
- Commit messages end with:
  `Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>`
- Deployment repo: `LuisFernandoRosa.github.io` (root-URL GitHub Pages). Enabling Pages is done **with the user**.

---

## File Structure

| File | Responsibility |
|---|---|
| `index.html` | Homepage: masthead + nav + Writing index (empty state for now). |
| `cv.html` | Curriculum page, Bevel-styled, experience-led. |
| `writing/.gitkeep` | Placeholder so the posts directory is tracked. |
| `assets/css/bevel.css` | All styles + design tokens in `:root` (single source). Add nav + CV component styles + token comments. |
| `assets/img/portrait.jpg` | Header portrait (already present). |
| `assets/Luis-Fernando-Rosa-Resume.pdf` | Downloadable résumé (copied into repo). |
| `docs/post-template.html` | Standalone per-post skeleton (from the reading-view sample). |
| `docs/design-system.md` | Unchanged — design source of truth. |
| `CLAUDE.md` | Simple project instructions. |
| `README.md` | How to run / add a post / where to change things. |
| `.nojekyll` | Tells GitHub Pages to serve the repo as-is. |
| `.gitignore` | OS/editor cruft (already present). |

---

### Task 1: Baseline — bring the existing prototype under version control

**Files:**
- Add (already on disk): `index.html`, `assets/css/bevel.css`, `assets/img/portrait.jpg`, `docs/design-system.md`, `README.md`

**Interfaces:**
- Produces: a committed baseline of the Bevel prototype that later tasks modify.

- [ ] **Step 1: Confirm working tree state**

Run: `git status --short`
Expected: `index.html`, `assets/`, `docs/design-system.md`, `README.md` show as untracked (`??`). The spec under `docs/superpowers/specs/` is already committed.

- [ ] **Step 2: Verify the prototype renders**

Run: `python3 -m http.server 8000` and open `http://localhost:8000/` (or open `index.html` directly). Confirm the masthead, portrait, and existing sample entries render. Stop the server.

- [ ] **Step 3: Stage the prototype files**

```bash
git add index.html assets/css/bevel.css assets/img/portrait.jpg docs/design-system.md README.md
```

- [ ] **Step 4: Commit**

```bash
git commit -m "chore: baseline Bevel prototype under version control

Existing static prototype (masthead, portrait, reading-view sample) and
the locked design system, committed as the starting point for Phase 1.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 2: Scaffold static-site plumbing

**Files:**
- Create: `.nojekyll`
- Create: `assets/Luis-Fernando-Rosa-Resume.pdf` (copy)
- Create: `writing/.gitkeep`

**Interfaces:**
- Produces: `.nojekyll` (GitHub Pages serves repo as-is); résumé asset path `assets/Luis-Fernando-Rosa-Resume.pdf` used by the CV download link (Task 6); `writing/` directory for Phase 2 posts.

- [ ] **Step 1: Create `.nojekyll` (empty file)**

```bash
touch .nojekyll
```

- [ ] **Step 2: Copy the résumé into the repo**

```bash
cp "/Users/luis/Downloads/Luis-Fernando-Rosa-Resume.pdf" assets/Luis-Fernando-Rosa-Resume.pdf
```
Expected: `ls assets/` shows `Luis-Fernando-Rosa-Resume.pdf`. If the source file is missing, ask the user for it before proceeding.

- [ ] **Step 3: Create the posts directory placeholder**

```bash
mkdir -p writing && touch writing/.gitkeep
```

- [ ] **Step 4: Commit**

```bash
git add .nojekyll assets/Luis-Fernando-Rosa-Resume.pdf writing/.gitkeep
git commit -m "chore: scaffold static-site plumbing (.nojekyll, resume asset, writing/)

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 3: Design-token config pass on `bevel.css`

**Files:**
- Modify: `assets/css/bevel.css:1-13` (the `:root` block — add orienting comments only; do not change values)

**Interfaces:**
- Produces: a clearly-labeled single source of design tokens that `CLAUDE.md` (Task 7) points to.

- [ ] **Step 1: Add a header comment above `:root` marking it as the single source**

Replace the top of `assets/css/bevel.css` (the `:root{` opening) so the file begins with:

```css
/* ============================================================
   BEVEL — design tokens (SINGLE SOURCE OF TRUTH)
   Change the accent color or fonts HERE and nowhere else.
   - Accent: --accent  (must pass shine rule: L >= 0.68, C >= 0.07)
   - Fonts:  --display / --body / --util
     NOTE: font *files* load via the Google Fonts <link> in the
     <head> of each HTML page — changing a typeface means updating
     BOTH the variable below AND that <link>.
   See docs/design-system.md §2 (colour) and §3 (typography).
   ============================================================ */
:root{
  --void:#151517;
  --surface:#1C1D20;
  --bone:#E8E5DE;
  --muted:#8B8880;
  --accent:#66BEAB;        /* chrysocolla */
  --accent-deep:#357669;
  --hair:#2C2D31;
  --display:"Instrument Serif",Georgia,serif;
  --body:"Archivo",system-ui,sans-serif;
  --util:"JetBrains Mono",ui-monospace,monospace;
  --portrait:url("../img/portrait.jpg");
}
```

- [ ] **Step 2: Verify no visual change**

Open `index.html` in the browser. Confirm the page looks identical to Task 1 (comments only, no value changes).

- [ ] **Step 3: Commit**

```bash
git add assets/css/bevel.css
git commit -m "docs(css): label :root as the single source for accent + fonts

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 4: Rework the homepage (nav + Writing empty state, remove fake content)

**Files:**
- Modify: `index.html` (replace the `<main>` sample entries and the reading-view `<section class="sample">`; add nav)
- Modify: `assets/css/bevel.css` (append nav + empty-state styles)

**Interfaces:**
- Consumes: `bevel.css` tokens/classes.
- Produces: `nav.topnav` markup pattern and `.empty` style reused conceptually on `cv.html` (Task 6); link to `cv.html`.

- [ ] **Step 1: Replace the homepage body content**

In `index.html`, replace everything from `<div class="section-head">` through the closing `</section>` of `class="sample"` (i.e. the fake entries and the reading sample) with the Writing index empty state below. Also add a nav right after `<div class="wrap">`'s opening, before `<header class="masthead">`. Final `<body>` inner structure:

```html
<div class="bevel" aria-hidden="true"></div>
<div class="bleed" aria-hidden="true"><div class="bleed-img"></div></div>

<div class="wrap">
  <nav class="topnav" aria-label="Primary">
    <a class="label" href="index.html" aria-current="page">Writing</a>
    <a class="label" href="cv.html">CV</a>
  </nav>

  <header class="masthead">
    <div class="label">AI Engineer · Multi-agent systems</div>
    <h1 class="namemark">Luis Fernando <em>da Rosa</em></h1>
    <p class="standfirst">I build multi-agent systems that write software. I also grind knives. The two turn out to be the same problem: knowing when something is finished.</p>
    <div class="edge-rule"></div>
  </header>

  <div class="section-head"><span class="label">Writing</span><span class="label">Soon</span></div>

  <main>
    <p class="empty">First entries are on the way. In the meantime, the <a href="cv.html">curriculum</a> is here.</p>
  </main>

  <footer>
    <span class="label">Luis Fernando da Rosa · 2026</span>
    <span class="label">RSS · GitHub · Email</span>
  </footer>
</div>
```

- [ ] **Step 2: Append nav + empty-state styles to `bevel.css`**

Add near the layout section (after the `.masthead` block, before `/* ---------- index ---------- */`):

```css
/* ---------- top nav ---------- */
.topnav{display:flex;gap:1.8rem;padding:3.2vh 0 0;align-items:baseline}
.topnav a{transition:color .45s ease}
.topnav a:hover,.topnav a:focus-visible{color:var(--bone)}
.topnav a[aria-current="page"]{color:var(--bone)}

/* ---------- empty state ---------- */
.empty{color:var(--muted);max-width:44ch;padding:1.4rem 0 2rem}
.empty a{color:var(--bone);text-decoration:underline;text-underline-offset:3px;text-decoration-color:var(--hair)}
.empty a:hover{text-decoration-color:var(--accent)}
```

- [ ] **Step 3: Verify the homepage**

Serve (`python3 -m http.server 8000`) and open `http://localhost:8000/`. Confirm:
- Nav shows "Writing" (current) and "CV" at top-left.
- Masthead and portrait unchanged.
- No fake posts, no reading sample; the empty-state line renders with a working link to `cv.html` (will 404 until Task 6 — that's expected).
- Resize to 320px: nav wraps/holds, no horizontal scroll.

- [ ] **Step 4: Commit**

```bash
git add index.html assets/css/bevel.css
git commit -m "feat: homepage nav + Writing empty state; remove placeholder content

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 5: Extract the post template

**Files:**
- Create: `docs/post-template.html` (standalone skeleton from the reading-view sample)

**Interfaces:**
- Produces: the reference skeleton Claude fills per post in Phase 2. Not linked from the live site.

- [ ] **Step 1: Create `docs/post-template.html`**

A complete standalone page reusing the Bevel prose styles. Placeholders in `{{ ... }}` are filled per post:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{{ POST TITLE }} · Luis Fernando da Rosa</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=Archivo:wght@300;400;500&family=JetBrains+Mono:wght@400&display=swap" rel="stylesheet">
<link rel="stylesheet" href="../assets/css/bevel.css">
</head>
<body>
<div class="bevel" aria-hidden="true"></div>

<div class="wrap">
  <nav class="topnav" aria-label="Primary">
    <a class="label" href="../index.html">Writing</a>
    <a class="label" href="../cv.html">CV</a>
  </nav>

  <article class="sample">
    <div class="label">Reading view</div>
    <h1 class="sample-head">{{ POST TITLE }}</h1>
    <div class="sample-meta"><span class="label">{{ DATE }} · {{ READ TIME }} · {{ CATEGORY }}</span></div>
    <div class="prose">
      <p>{{ First paragraph. }}</p>
      <div class="pull">{{ Optional pull quote — display serif, one idea. }}</div>
      <p>{{ More prose. Code blocks: --bone on --surface, NO syntax colour. Diagrams: inline SVG only, no JS. }}</p>
    </div>
  </article>

  <footer>
    <span class="label">Luis Fernando da Rosa · 2026</span>
    <span class="label">RSS · GitHub · Email</span>
  </footer>
</div>
</body>
</html>
```

- [ ] **Step 2: Verify it renders**

Open `docs/post-template.html` in the browser. Confirm prose, pull-quote, and heading styles apply (CSS path `../assets/css/bevel.css` resolves). The bevel shows; no portrait (posts don't use it).

- [ ] **Step 3: Commit**

```bash
git add docs/post-template.html
git commit -m "docs: add per-post HTML template from the reading-view sample

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 6: Build the CV page

**Files:**
- Create: `cv.html`
- Modify: `assets/css/bevel.css` (append CV component styles)

**Interfaces:**
- Consumes: `bevel.css` tokens/classes, `.topnav`, résumé asset from Task 2.
- Produces: `cv.html` linked from the homepage nav.

**Copy is final (rewritten from the résumé; no new claims).**

- [ ] **Step 1: Append CV component styles to `bevel.css`**

Add at the end (before the media queries, or after them — keep the `@media` blocks last is fine; append before `@media (max-width:680px)`):

```css
/* ---------- cv ---------- */
.cv-lede{max-width:46ch;margin-top:1.9rem;font-size:1.14rem;line-height:1.55;color:var(--muted)}
.cv-contact{display:flex;flex-wrap:wrap;gap:1.4rem;margin-top:1.6rem}
.cv-contact a{color:var(--bone);text-decoration:underline;text-underline-offset:3px;text-decoration-color:var(--hair)}
.cv-contact a:hover{text-decoration-color:var(--accent)}

.cv-section{padding:8vh 0 0}
.cv-section > .label{display:block;margin-bottom:2rem}

.role{display:grid;grid-template-columns:150px 1fr;gap:2.4rem;padding:2rem 0;border-bottom:1px solid var(--hair)}
.role-side .label{display:block}
.role-side .label + .label{margin-top:.45rem;color:var(--accent-deep)}
.role-title{font-family:var(--display);font-weight:400;font-size:clamp(1.4rem,3vw,1.9rem);line-height:1.15}
.role-bullets{list-style:none;margin-top:.8rem}
.role-bullets li{color:var(--muted);max-width:64ch;line-height:1.55;padding-left:1.1rem;position:relative;margin-top:.7rem}
.role-bullets li::before{content:"";position:absolute;left:0;top:.62em;width:5px;height:1px;background:var(--accent-deep)}

.toolkit{display:grid;grid-template-columns:150px 1fr;gap:2.4rem;padding:1.4rem 0;border-bottom:1px solid var(--hair)}
.toolkit dt{grid-column:1}
.toolkit dd{grid-column:2;color:var(--muted);max-width:64ch;line-height:1.5}
.toolkit-grid{display:contents}

.project-note{color:var(--muted);max-width:60ch;margin-top:1rem;line-height:1.6}
.project-note a{color:var(--bone);text-decoration:underline;text-underline-offset:3px;text-decoration-color:var(--hair)}
.project-note a:hover{text-decoration-color:var(--accent)}

.cv-download{margin-top:6vh}
.cv-download a{color:var(--bone);text-decoration:underline;text-underline-offset:3px;text-decoration-color:var(--accent-deep)}
.cv-download a:hover{text-decoration-color:var(--accent)}

@media (max-width:680px){
  .role,.toolkit{grid-template-columns:1fr;gap:.8rem}
  .role-side{display:flex;gap:1.4rem}
  .role-side .label + .label{margin-top:0}
  .toolkit dt{margin-bottom:.3rem}
}
```

Note: the existing `@media (max-width:680px)` block stays; this adds a second one, which is valid CSS. Optionally merge by hand.

- [ ] **Step 2: Create `cv.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>CV · Luis Fernando da Rosa</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=Archivo:wght@300;400;500&family=JetBrains+Mono:wght@400&display=swap" rel="stylesheet">
<link rel="stylesheet" href="assets/css/bevel.css">
</head>
<body>
<div class="bevel" aria-hidden="true"></div>

<div class="wrap">
  <nav class="topnav" aria-label="Primary">
    <a class="label" href="index.html">Writing</a>
    <a class="label" href="cv.html" aria-current="page">CV</a>
  </nav>

  <header class="masthead">
    <div class="label">AI/ML Engineer · Agentic Systems · Production AI from 0→1</div>
    <h1 class="namemark">Luis Fernando <em>da Rosa</em></h1>
    <p class="cv-lede">I build AI systems that ship, not prototypes that stall. Six years across agentic systems, LLM pipelines, and computer vision — designing multi-agent architectures, owning technical decisions end to end, and navigating the constraints that decide whether AI reaches production: cost, latency, security, and messy data.</p>
    <div class="cv-contact">
      <a href="https://www.linkedin.com/in/luisfernandodarosa">LinkedIn</a>
      <a href="https://www.linkedin.com/in/luisfernandodarosa">LinkedIn</a>
      <a href="https://github.com/LuisFernandoRosa">GitHub</a>
      <span class="label">Paraná, Brazil</span>
    </div>
    <div class="edge-rule"></div>
  </header>

  <section class="cv-section" aria-label="Experience">
    <span class="label">Experience</span>

    <div class="role">
      <div class="role-side"><span class="label">Jun 2026 — Present</span><span class="label">Golabs Tech</span></div>
      <div>
        <h2 class="role-title">AI Engineer</h2>
        <ul class="role-bullets">
          <li>Architected a multi-agent engineering system inside a private payments codebase — specialist agents for planning, backend, frontend, API, and documentation, with a router that classifies each request and dispatches it to the right one.</li>
          <li>Built the shared framework standardizing tooling, state, and execution contracts, so adding a new agent no longer means rebuilding scaffolding.</li>
          <li>Designed the eval harness scoring every agent on correctness, run-to-run consistency, and cost per task — and used the numbers to pick each agent's backing model; instrumented end-to-end tracing to locate failures at the exact step that broke.</li>
        </ul>
      </div>
    </div>

    <div class="role">
      <div class="role-side"><span class="label">Jun 2025 — May 2026</span><span class="label">Arionkoder</span></div>
      <div>
        <h2 class="role-title">AI Engineer</h2>
        <ul class="role-bullets">
          <li>Invented a <code>bot_id</code> HTML-injection technique letting LLMs identify semantic regions without hand-crafted selectors — the core IP of the extraction product, generalizing across 10+ structurally diverse site templates.</li>
          <li>Designed a 4-stage pipeline (region discovery → extraction → structuring → LLM-as-judge) that automated CMS population, cutting manual entry from hours to minutes per page while lowering per-run token cost.</li>
        </ul>
      </div>
    </div>

    <div class="role">
      <div class="role-side"><span class="label">Nov 2023 — May 2025</span><span class="label">Flow+</span></div>
      <div>
        <h2 class="role-title">Senior AI Engineer</h2>
        <ul class="role-bullets">
          <li>Led the company's flagship AI product: a developer-burnout detector ingesting Bitbucket and Jira telemetry, surfacing risk signals 2–3 weeks earlier than traditional indicators.</li>
          <li>Owned AI delivery end to end across healthcare and productivity domains — architecture through deployment — with full technical ownership and no dedicated engineering manager.</li>
        </ul>
      </div>
    </div>

    <div class="role">
      <div class="role-side"><span class="label">Oct 2024 — Jan 2025</span><span class="label">Freelance</span></div>
      <div>
        <h2 class="role-title">AI Engineer</h2>
        <ul class="role-bullets">
          <li>Delivered a RAG pipeline (ChromaDB, LangChain) over financial and inventory corpora, turning hours of manual review into second-scale queries; automated classification, entity extraction, and summarization.</li>
        </ul>
      </div>
    </div>

    <div class="role">
      <div class="role-side"><span class="label">Feb 2023 — Dec 2023</span><span class="label">Pix Force</span></div>
      <div>
        <h2 class="role-title">Computer Vision Engineer</h2>
        <ul class="role-bullets">
          <li>Shipped a LiDAR point-cloud system (Open3D, Trimesh) for coal-silo volume at &lt;5% error, replacing a dangerous manual survey.</li>
          <li>Built and deployed an OCR + GPT document-analysis system in Docker, cutting processing time ~70% and letting a client handle 3× the volume at the same headcount.</li>
        </ul>
      </div>
    </div>

    <div class="role">
      <div class="role-side"><span class="label">May 2022 — Jan 2023</span><span class="label">GETTER</span></div>
      <div>
        <h2 class="role-title">Computer Vision Engineer</h2>
        <ul class="role-bullets">
          <li>Deployed vision models to IoT edge devices at &lt;200ms latency on resource-constrained hardware, enabling a use case previously impossible on-device.</li>
          <li>Standardized the team's model delivery with Docker pipelines, eliminating ad-hoc deployment failures.</li>
        </ul>
      </div>
    </div>

    <div class="role">
      <div class="role-side"><span class="label">Sep 2020 — Sep 2022</span><span class="label">LAMIA — UTFPR</span></div>
      <div>
        <h2 class="role-title">Computer Vision Lead &amp; Researcher</h2>
        <ul class="role-bullets">
          <li>Led a 5-person CV research team across 3 projects, including Wood-Inspector — a wood-quality inspection system for the Amazon basin, patent-documented.</li>
          <li>Managed a bare-metal Kubernetes cluster supporting concurrent training workloads on a zero managed-infrastructure budget.</li>
        </ul>
      </div>
    </div>

    <div class="role">
      <div class="role-side"><span class="label">Jul 2021 — Apr 2022</span><span class="label">Silicon Village</span></div>
      <div>
        <h2 class="role-title">Software Engineer</h2>
        <ul class="role-bullets">
          <li>Built an event-driven serverless backend (TypeScript, AWS Lambda, Firebase Functions) that absorbed traffic spikes with zero infrastructure-management overhead; automated deployments with Docker and Kubernetes.</li>
        </ul>
      </div>
    </div>
  </section>

  <section class="cv-section" aria-label="Technical toolkit">
    <span class="label">Technical toolkit</span>
    <dl class="toolkit-grid">
      <div class="toolkit"><dt class="label">Agents &amp; LLMs</dt><dd>LangGraph, LangChain, OpenAI &amp; Azure OpenAI, Gemini, Claude / Claude Code, RAG, LLM-as-judge, structured outputs, prompt engineering</dd></div>
      <div class="toolkit"><dt class="label">Agent reliability</dt><dd>Multi-agent orchestration, request routing, eval harnesses (accuracy / consistency / cost), end-to-end tracing, model selection</dd></div>
      <div class="toolkit"><dt class="label">ML &amp; CV</dt><dd>TensorFlow, PyTorch, Keras, OpenCV, ONNX, Open3D &amp; Trimesh (point clouds), Tesseract OCR</dd></div>
      <div class="toolkit"><dt class="label">Data &amp; storage</dt><dd>Pandas, NumPy, scikit-learn, ChromaDB, vector search, Firebase</dd></div>
      <div class="toolkit"><dt class="label">Infrastructure</dt><dd>Docker, Kubernetes (bare-metal), AWS Lambda, Azure, Linux, Git/GitHub automation · Python, TypeScript, Node.js</dd></div>
    </dl>
  </section>

  <section class="cv-section" aria-label="Notable project">
    <span class="label">Notable project</span>
    <h2 class="role-title">Wood-Inspector Amazônia</h2>
    <p class="project-note">Advanced computer-vision system for wood quality inspection and classification, deployed in the Amazon basin. Patent documented. <a href="https://clb.lamia.sh.utfpr.edu.br">clb.lamia.sh.utfpr.edu.br</a></p>
    <p class="cv-download"><a href="assets/Luis-Fernando-Rosa-Resume.pdf">Download résumé (PDF)</a></p>
  </section>

  <footer>
    <span class="label">Luis Fernando da Rosa · 2026</span>
    <span class="label">RSS · GitHub · Email</span>
  </footer>
</div>
</body>
</html>
```

- [ ] **Step 3: Verify the CV page**

Serve and open `http://localhost:8000/cv.html`. Confirm:
- Nav shows "CV" as current; "Writing" links back to `index.html`.
- Masthead: positioning label, name with accent italic surname, lede, contact links, edge rule.
- All 8 roles render in reverse-chronological order with the date/company rail and accent-deep tick bullets.
- Toolkit groups align in the 150px + 1fr grid.
- "Download résumé (PDF)" link opens `assets/Luis-Fernando-Rosa-Resume.pdf`.
- **Accent discipline:** the only accent (chrysocolla) elements are the bevel, the italic surname, and hover/rule hairlines — no second accent introduced.
- Resize to 320px: role/toolkit grids collapse to one column, no horizontal scroll.
- Tab through: focus outline visible on nav and links.

- [ ] **Step 4: Commit**

```bash
git add cv.html assets/css/bevel.css
git commit -m "feat: add experience-led CV page in the Bevel style

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 7: CLAUDE.md and README

**Files:**
- Create: `CLAUDE.md`
- Modify: `README.md` (replace with current, accurate instructions)

**Interfaces:**
- Consumes: everything above (paths, workflow).
- Produces: the project's entry-point docs.

- [ ] **Step 1: Create `CLAUDE.md`**

```markdown
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

## Run it

No build step, no dependencies.

    python3 -m http.server 8000

…or just open `index.html`. **No client-side JavaScript — keep it that way.**

## Add a blog post

Write the post in Markdown and hand it over. It gets rendered into a
standalone Bevel HTML page in `writing/` following `docs/post-template.html`,
and linked from the homepage index. Code blocks are `--bone` on `--surface`
with no syntax colouring; diagrams are inline SVG (no JS).

## Deploy

GitHub Pages serves the repo directly (`.nojekyll`). Target repo:
`LuisFernandoRosa.github.io` (root URL). Push to the default branch = live.

## Source of truth

`docs/design-system.md` — tokens, rules, components, constraints. Read it
before changing anything visual; the values that look arbitrary aren't.
```

- [ ] **Step 2: Replace `README.md`**

```markdown
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
```

- [ ] **Step 3: Verify**

Confirm `CLAUDE.md` and `README.md` render on GitHub-flavored Markdown (headings, code fences intact). Confirm the paths referenced exist.

- [ ] **Step 4: Commit**

```bash
git add CLAUDE.md README.md
git commit -m "docs: add CLAUDE.md and rewrite README for the static site

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 8: Deployment to GitHub Pages (with the user)

**Files:** none (repo/remote operations).

**Interfaces:**
- Consumes: the committed site.
- Produces: a live site at `https://luisfernandorosa.github.io`.

> This task changes outward-facing state. Do it **with the user**; do not create accounts. Use `gh` only if authenticated.

- [ ] **Step 1: Final local verification**

Serve and click through `index.html` ↔ `cv.html`, the résumé download, and the empty-state link. Confirm no console-independent breakage (no JS anyway) and no horizontal scroll at 320px.

- [ ] **Step 2: Confirm/replace remote naming with the user**

Ask the user to confirm the repo name `LuisFernandoRosa.github.io` (root-URL user site). If they prefer a project repo, note the site will live under a subpath and links (all relative already) still work.

- [ ] **Step 3: Create the remote and push (if `gh` is available)**

```bash
gh repo create LuisFernandoRosa.github.io --public --source=. --remote=origin --push
```
If `gh` is not installed/authenticated, hand the user the manual equivalent:

```bash
git remote add origin git@github.com:LuisFernandoRosa/LuisFernandoRosa.github.io.git
git push -u origin main
```

- [ ] **Step 4: Enable Pages**

Direct the user to GitHub → repo Settings → Pages → Build and deployment → Source: **Deploy from a branch**, branch `main` / root. Save. Confirm the site loads at `https://luisfernandorosa.github.io` after the first build.

---

## Self-Review

**Spec coverage:**
- §1 scope → Tasks 1–8. ✓
- §2 architecture (static, no JS, `.nojekyll`) → Task 2 + Global Constraints. ✓
- §3 structure → Tasks 1–7 create every listed file. ✓
- §4 single-source config → Task 3 (tokens) + Task 7 (documented). ✓
- §5.1 homepage → Task 4. ✓  §5.2 CV → Task 6. ✓
- §6 design compliance → Global Constraints + verification steps in Tasks 4/6. ✓
- §7 git/commits → each task commits; sequence matches. ✓
- §8 deployment → Task 8. ✓
- §9 CLAUDE.md → Task 7. ✓
- §10 success criteria → covered by verification steps.

**Placeholder scan:** The only `{{ }}` placeholders are inside `docs/post-template.html`, which is intentionally a fill-in template (not a plan gap). No TBD/TODO in build tasks.

**Type/name consistency:** Class names used consistently — `.topnav`, `.empty`, `.cv-lede`, `.cv-contact`, `.cv-section`, `.role`/`.role-side`/`.role-title`/`.role-bullets`, `.toolkit`/`.toolkit-grid`, `.project-note`, `.cv-download` are all defined in Task 6's CSS (or Task 4's for `.topnav`/`.empty`) and referenced in the matching HTML. Résumé path `assets/Luis-Fernando-Rosa-Resume.pdf` matches between Task 2 (create) and Task 6 (link).
```
