# Portfolio Dev Log — zhafransyahh.github.io

A full record of design decisions, technical changes, errors encountered, and the reasoning behind them.

---

## Stack

- **Vanilla HTML5 / CSS3 / JS** — no frameworks, no build tools
- **Google Fonts:** Fraunces (serif headlines), DM Sans (body), Space Mono (labels, nav, tags)
- **Hosting:** GitHub Pages from `master` branch
- **Repo:** `zhafransyahh/zhafransyahh.github.io`

---

## Design System

Defined in `css/main.css` as CSS custom properties:

```css
:root {
    --accent: #586994;         /* Baltic Blue — primary brand color */
    --accent-hover: #6b7db0;
    --bg: #0c1220;             /* near-black navy background */
    --bg-subtle: #111827;
    --bg-card: #151d2e;
    --text-primary: #e4e8ed;   /* cool off-white */
    --text-secondary: #8892a4;
    --text-muted: #4a5568;
    --border: rgba(228,232,237,0.08);
    --serif: 'Fraunces', Georgia, serif;
    --sans: 'DM Sans', system-ui, sans-serif;
    --mono: 'Space Mono', 'Courier New', monospace;
}
```

**Theme:** Dark editorial. Navy-black background, cool off-white type, Baltic Blue accent.

---

## Typography Decision

### Problem
Original font stack used **Instrument Serif + Space Mono**. Space Mono was the body font — hard to read at paragraph length due to its fixed-width letterforms.

### Options considered
- **Option A:** Keep Instrument Serif, replace body with Inter — safe but generic
- **Option B:** Switch to Playfair Display + Lato — classic editorial, less distinctive
- **Option C:** Fraunces + DM Sans + keep Space Mono for labels only

### Decision: Option C
- **Fraunces** — optical-size aware serif, natural italic, distinctive without being novelty. Headlines only.
- **DM Sans** — humanist sans-serif, high readability at body sizes. Body text.
- **Space Mono** — kept strictly for nav, labels, tags, timestamps. Its rigidity works for small metadata.

---

## Hero Section

### Profile photo — tried in hero, reverted to About
Moved profile photo into hero as two-column grid. Didn't look good — disrupted editorial single-column feel. Reverted to About section.

Settings kept: `380px` wide, `aspect-ratio: 3/4`, `border-radius: 16px`, `filter: grayscale(20%) brightness(1.05)`, `object-position: center top`.

### Scroll indicator animation removed
`.hero__scroll` pulse/sway keyframe animation removed. Distracting, serves no functional purpose. Static `opacity: 0.5`.

---

## Experience Section

- **Metric cards** — key numbers in card grid. Value in Fraunces serif at 2rem (accent color), label in DM Sans 0.75rem.
- **Company name** — Space Mono, 0.6875rem, text-secondary.
- **Bullet size** — increased from 0.8125rem to 0.9375rem.
- **Item spacing** — padding-bottom increased from 3rem to 4rem.

### Show Earlier Roles Toggle

**Problem:** CSS transitions don't animate when `hidden` attribute is toggled (applies `display: none` instantly).

**Fix:** CSS `max-height` + `opacity` transition controlled by `.is-open` class:
```css
.timeline__more {
    max-height: 0; overflow: hidden; opacity: 0;
    transition: max-height 0.5s cubic-bezier(0.4, 0, 0.2, 1), opacity 0.4s ease;
}
.timeline__more.is-open { max-height: 800px; opacity: 1; }
```

Toggle button alignment: `margin-left: 1rem` puts it at `2rem (timeline padding) + 1rem = 3rem` from section edge, matching text content start.

---

## Contact Nav Highlight Bug

**Problem:** IntersectionObserver with `rootMargin: '-80px 0px -40% 0px'` clips 40% from viewport bottom. Contact section is short and at the very bottom — observer never fires.

**Fix:** Supplementary scroll listener:
```js
window.addEventListener('scroll', () => {
    const atBottom = window.innerHeight + window.scrollY >= document.body.scrollHeight - 10;
    if (atBottom) setActive('contact');
});
```

---

## Case Study Pages — Legacy (Gitra, JurnalKamu, Helm-In)

These pages are self-contained with inline `<style>` blocks — they do NOT import `css/main.css`. Each has its own design token set.

**Hero restructure:** Removed inner `hero-nav` (squashed nav-bar look). Moved case study label to `hero-eyebrow`. Project name became the main `hero-title`.

**Text overflow fix:** Added `section > * { max-width: 1100px; margin: auto; }`.

**Mobile optimization:** `100dvh` for iOS, `env(safe-area-inset-*)` for notch, `-webkit-tap-highlight-color: transparent`, `touch-action: manipulation`, `overscroll-behavior-y: none`.

---

## iOS & Android Mobile Optimization

| Issue | Fix |
|---|---|
| `100vh` bug on iOS | `@supports (min-height: 100dvh) { .hero { min-height: 100dvh; } }` |
| Content behind notch | `env(safe-area-inset-*)` on nav and hero padding |
| Auto-zoom on sub-16px text | `-webkit-text-size-adjust: 100%` |
| Blue tap flash | `-webkit-tap-highlight-color: transparent` |
| 300ms tap delay | `touch-action: manipulation` |
| Pull-to-refresh interrupts nav | `overscroll-behavior-y: none` |
| Hover states stick on touch | Wrapped hover effects in `@media (hover: hover)` |
| Touch feedback on work cards | `@media (hover: none) { .work__item:active { background: ... } }` |

---

## Git Setup

Branch named `main` locally, GitHub Pages served from `master`.
```bash
git branch -m main master
git push origin master --force
git push origin --delete main
```

---

---

# Session 2 — JULO PM Case Studies Build + Redesign

## What was built

Three professional PM case study HTML pages for JULO work:

| File | Title | Subject |
|---|---|---|
| `cs-lender-integration.html` | No Map of the Territory | Institutional lender API integration, inherited mid-project |
| `cs-maps-cost.html` | Invisible at Low Volume | Google Maps API 99% cost reduction via UX fix |
| `cs-insurance-claims.html` | The Arithmetic of Trust | Insurance claims system, 0-to-1 build |

And `gitra-project.html` was given a new evocative title: **What the Hands Already Know**.

---

## New CSS File: `css/case-study.css`

Created a shared layout stylesheet for the three JULO case study pages. Both `css/main.css` (for design tokens) and `css/case-study.css` (for layout) are linked on each PM case study page.

Key classes:
- `.cs-nav` — fixed nav with glassmorphism on scroll (`.cs-nav--scrolled`)
- `.cs-hero` — 80vh min-height hero with `.cs-hero__eyebrow` and `.cs-meta` 4-cell grid
- `.cs-body` — max-width prose wrapper
- `.cs-section` — content section with border-bottom dividers
- `.cs-diagram` — diagram/chart container with label
- `.cs-impact` — large serif metric callouts grid
- `.cs-takeaway` — closing quote section
- `.cs-footer` — prev/next case study navigation

**Max-widths in `css/case-study.css`:**
- `.cs-hero__title`: `max-width: 1320px`
- `.cs-meta`, `.cs-body`, `.cs-impact__inner`, `.cs-takeaway__inner`: `max-width: 1140px`

---

## Per-Page Accent Colors

Each PM case study sets `--page-accent` via a `<style>` block on `<body>`. This scopes the accent color to diagrams, highlights, and chart fills without changing the global design system.

```css
/* cs-lender-integration.html */
body { --page-accent: #586994; } /* Baltic Blue — institutional */

/* cs-maps-cost.html */
body { --page-accent: #3d8c6e; } /* Teal — optimization/savings */

/* cs-insurance-claims.html */
body { --page-accent: #7b5ea7; } /* Indigo — regulatory/systematic */
```

---

## Diagrams — Pure HTML/CSS/SVG, No JavaScript

All data visualizations are static (no chart libraries). Reasons:
1. No external dependencies
2. Renders without JS
3. Fully controllable with CSS custom properties for theming

### cs-lender-integration.html diagrams

**Project timeline** — horizontal SVG with 8 milestones. The mid-stream pivot milestone has a warning-style visual break to show the architectural pivot point.

**Money flow diagram** — CSS flexbox boxes with SVG arrow connectors. Two paths: Lender → JULO → Borrower (disbursement, API), and Borrower → JULO → Lender (repayment, SFTP).

**Share chart** — horizontal bar chart showing institutional lender channel's share of total monthly platform disbursement since go-live. Uses CSS custom properties (`--w`) for bar widths. Animated on scroll via IntersectionObserver:
```js
const shareObs = new IntersectionObserver((entries) => {
    entries.forEach(e => {
        if (e.isIntersecting) {
            shareChart.classList.add('animated');
            shareObs.unobserve(e.target);
        }
    });
}, { threshold: 0.3 });
```
Bars start at `width: 0`, transition to `var(--w)` when `.animated` class is applied. `prefers-reduced-motion` check: bars render at full width immediately.

**6-stat subgrid** — 2×3 grid of supporting stats. Chosen as 6 (not 5) because a 3-column grid with an odd number leaves an orphaned cell.

### cs-maps-cost.html diagrams

**API spike bar chart** — CSS bar chart. X-axis: weeks. Y-axis: relative API volume. Three visual zones: baseline, spike (5,600% annotated), post-fix. Animated on scroll.

**UX flow: Before / After** — two-column side-by-side screen mockup cards. Before: immediate API trigger on address field focus. After: manual tap into map picker, 1 call on confirm.

### cs-insurance-claims.html diagrams

**4-step loan lifecycle stepper** — horizontal stepper with SVG glyphs, titles, and 1-line descriptions for: Enrollment → Premium Calculation → Claim Trigger → Claim Confirmation.

**3-party architecture diagram** — hub-and-spoke showing JULO (center) ↔ Insurance Partner ↔ Institutional Lender. Labeled edges for each data/money flow.

**Processing time roadmap** — simple two-bar visual: current (7 days) vs. target (1 day via automation).

---

## NDA Compliance — Anonymizing the Bank Partner

The institutional lender was originally referred to as "DBS" throughout `cs-lender-integration.html`. Changed to "institutional lender" at the user's request to avoid naming the bank.

**Changes made:**
- All body text references: "DBS channel" → "institutional lender channel"
- Diagram labels updated
- CSV column name in disclaimer: `j1_dbs_chnl_amt` → `lender_channel_amt` (the internal column name would have identified the partner)
- Meta description and eyebrow updated

---

## Metrics Verification (cs-lender-integration.html)

Metrics on the case study were verified against internal CSV data. The share chart and stat subgrid were updated based on confirmed figures:

| Stat | Value | Source |
|---|---|---|
| Peak channel share (by amount) | ~40% | lender_channel_amt ÷ platform_total_amt |
| Share by loan count (Q1 2026) | ~33% | monthly breakdown |
| Average ticket size vs. platform | +17% | per-loan amount comparison |
| Platform-wide repeat borrower rate | ~93% | borrower cohort data |
| Channel variance (last 3 months) | ±5% | monthly share tracking |
| Ramp time to 1-in-3 loans | 6 months | go-live to milestone |

The impact strip first card was updated from "2×" to "~40%" to reflect the verified CSV figure (peak share, not a multiplier).

---

## Global Font Scale: 150% Zoom Effect

**Problem:** User wanted the site to appear at ~150% browser zoom at native (100%) zoom level.

**Rejected approaches:**
- CSS `zoom` property — causes horizontal overflow on wide containers
- `transform: scale()` on body — breaks fixed-position nav

**Solution:** Scale `html { font-size }` from `16px` to `24px`. Since all layout values use `rem`, this scales everything proportionally. Then manually scaled all `px`-based max-widths and grid columns.

**Changes in `css/main.css`:**
```css
html { font-size: 24px; }  /* was 16px */

/* Max-widths scaled ×1.5 */
.hero__subtitle { max-width: 960px; }         /* was 640px */
.philosophy { max-width: 1140px; }            /* was 760px */
.writing__block { max-width: 840px; }         /* was 560px */
.page-header__desc { max-width: 840px; }      /* was 560px */
.about__portrait img { max-width: 630px; }    /* was 420px */
.timeline__desc { max-width: 900px; }         /* was 600px */
.work__desc, .work__problem { max-width: 630px; }  /* was 420px */

/* Grid columns changed to clamp() for fluid scaling */
.about__grid { grid-template-columns: clamp(300px, 40%, 630px) 1fr; }
/* @media 768px work__item */ grid-template-columns: clamp(280px, 38%, 510px) 1fr;
/* @media 1000px work__item */ grid-template-columns: clamp(350px, 40%, 630px) 1fr;
```

**Changes in `css/case-study.css`:**
```css
.cs-hero__title { max-width: 1320px; }  /* was 880px */
.cs-meta { max-width: 1140px; }         /* was 760px */
.cs-body { max-width: 1140px; }         /* was 760px */
.cs-impact__inner { max-width: 1140px; } /* was 760px */
.cs-takeaway__inner { max-width: 1140px; } /* was 760px */
```

**Why clamp() for grid columns:** Fixed px grid columns (e.g., `420px 1fr`) at a 24px base become too wide relative to the 768px breakpoint. `clamp(min, %, max)` keeps them fluid.

---

## Universal Framing — Removing Fintech Focus

**Problem:** Portfolio was explicitly fintech-focused. Recruiters from other industries would find it inaccessible.

**Decision:** Reframe around transferable PM skills. Never mention the industry context in a way that excludes non-fintech readers.

**Changes across all pages:**

`index.html`:
- Hero subtitle: "fintech...lending" → "hard system problems: large-scale integrations, infrastructure cost breakdowns, 0-to-1 builds"
- Approach paragraph: "In fintech" → "In high-stakes environments"; "bank integration" → "enterprise integration"
- Impact strip #1: "2× / Lender fund availability" → "~40% / Platform channel growth"
- DBS work card: tag "Fintech" → "Technical PM"
- Insurance work card: tags "Insurance/0-to-1" → "0-to-1 Build/System Design"
- About: "fintech lending platforms" → "high-growth technology platforms in Southeast Asia"

`portfolio.html`:
- DBS tags: "Fintech/Institutional Lending" → "Technical PM/Enterprise Integration"
- Insurance problem text: "Institutional lending requires insurance coverage" → "A product launch creates a new compliance obligation"
- Insurance tags: → "0-to-1 Build/System Design/Regulatory Product"

Case study pages:
- `cs-lender-integration.html` eyebrow: "Institutional Lending" → "Enterprise Integration"
- `cs-maps-cost.html` eyebrow: "Fintech" → "Cost Optimization"
- `cs-insurance-claims.html` eyebrow: "Institutional Lending" → "0-to-1 System Design"

---

## Evocative Case Study Titles

**Inspiration:** Dario Amodei's essay "Machines of Loving Grace." The title works because it's paradoxical, literary, and rewards a second read. Goal: titles that aren't descriptive labels but carry the weight of the case study's core insight.

### Titles and rationale

**"No Map of the Territory"** (`cs-lender-integration.html`)
The case study is explicitly about operating without documentation — inheriting a mid-project integration with no handover, no prior architect. Inverts Korzybski's "the map is not the territory" — here, there is no map and the territory is live production infrastructure.

**"Invisible at Low Volume"** (`cs-maps-cost.html`)
The entire case study turns on a single insight: the UX flaw always existed, it just became catastrophic at scale. The spike was not caused by a new bug — growth revealed a flaw that was invisible before. Functions as a general principle: many PM problems are of this class.

**"The Arithmetic of Trust"** (`cs-insurance-claims.html`)
Insurance claims at the intersection of lending and regulation is fundamentally a precision problem. "Arithmetic" signals systems thinking; "Trust" signals human stakes. Mirrors the cold/warm structure of "Machines of Loving Grace."

**"What the Hands Already Know"** (`gitra-project.html`)
Gitra teaches guitar without visual feedback — through touch, sound, haptics. The design insight: hands have embodied knowledge. The app doesn't replace sight; it routes learning through what hands already know. Also gestures at accessibility as design intelligence, not accommodation.

### Implementation
Each title was applied in three places per project:
1. `<title>` tag — e.g., `No Map of the Territory | Rafi Zhafransyah`
2. `cs-hero__title` H1 — e.g., `No Map of<br>the <em>Territory</em>`
3. Work card `<h3>` in both `index.html` and `portfolio.html`

For Gitra: "Gitra" moved from H1 to the eyebrow (`Gitra: UX / UI Case Study`) so the app name stays discoverable.

---

## Nav Update — index.html

**Before:** 3 links — About, Work, Contact

**After:** 5 links — Approach, Work, About, Writing, Contact

Reason: `index.html` has 5 named anchor sections. The nav should match them so readers can jump to any section. `#impact` was excluded because it's a compact metrics strip embedded between Approach and Work, not a standalone navigable section.

---

## Em Dash Removal

### Step 1: Replace with hyphens
Global sed across all HTML files:
```bash
sed -i 's/ &mdash; / - /g; s/&mdash;/-/g; s/ — / - /g; s/—/-/g' *.html
```

### Step 2: Context-sensitive replacement of hyphens
After reviewing all occurrences, replaced separator hyphens with appropriate punctuation:

| Context | Replacement | Example |
|---|---|---|
| `<title>` and OG title tags | ` \| ` | `About \| Rafi Zhafransyah` |
| Section/diagram labels | `: ` | `Project milestones: Early 2024` |
| List item term + definition | `: ` | `Data mapping: aligning our internal data models` |
| Numbered section labels | `. ` | `01. Overview` |
| Prose soft break (additive) | `, ` | `strategic priority, diversifying our lending capacity` |
| Prose strong break (clause) | `; ` | `my ownership; I monitor it` |
| Heading sub-phrase | `, ` or `: ` | `funding gap, and no safety net` |
| Date ranges | unchanged | `Oct 2023 - Present` |
| CSS calc() expressions | unchanged | `calc(50% - 550px)` |
| Version labels | `: ` | `v1.0: Initial` |
| Company, Location | `, ` | `JULO, Jakarta` |

Total: ~100 individual replacements across 9 HTML files. Applied via targeted sed commands per-file.

---

## portfolio.html — Professional Work Section

Added a "Professional Work: JULO (2024)" section above the existing Gitra section. Three `.work__item` cards using the same pattern as the existing work grid.

Card structure:
```html
<a class="work__item reveal" href="cs-lender-integration.html">
    <div class="work__meta">
        <span class="featured__label">Product Manager · Enterprise Integration</span>
        <h3 class="work__title">No Map of the Territory</h3>
        <p class="work__problem">...</p>
        <p class="work__role">PM · Integration · Money Flow Design</p>
        <p class="work__outcome">~40% of total monthly platform volume · SLA-exceeding uptime since go-live</p>
        <div class="work__tags">...</div>
        <span class="work__arrow">Read case study →</span>
    </div>
</a>
```

---

## index.html — Work Section

Added 3 JULO work cards above the existing Gitra featured card. Added sub-labels:
- "Professional Work: JULO" (above the 3 PM cards)
- "Academy Work: Apple Developer Academy" (above Gitra)

Both sub-labels use `section__sublabel` with inline monospace styling consistent with the existing label system.

---

## Errors Encountered This Session

### Edit tool "file not read" error
**Cause:** Attempted to use Edit tool on `cs-maps-cost.html` and `cs-insurance-claims.html` without reading them first in the current session context.
**Fix:** Always read the file first (even just the first 30 lines is sufficient to establish context). The Edit tool requires file content to be in context for exact string matching.

### 6-stat grid layout
**Problem:** Expanding the DBS case study subgrid from 3 stats to 5 would leave an orphaned cell in a 3-column grid.
**Fix:** Added a 6th meaningful stat ("6 months ramp time") to complete a clean 2×3 grid.

### Grid columns too wide after font scale
**Problem:** After changing `html { font-size }` to 24px, fixed-px grid columns (e.g., `420px 1fr`) became disproportionately wide relative to the 768px breakpoint.
**Fix:** Changed to `clamp(min, %, max)` to keep columns fluid: `clamp(300px, 40%, 630px) 1fr`.

---

## File Structure (current state)

```
/
├── index.html                   # Homepage
├── about.html                   # Extended CV / about page
├── portfolio.html               # Portfolio listing
├── 404.html                     # Custom 404
├── cs-lender-integration.html      # PM case study: lender integration
├── cs-maps-cost.html            # PM case study: Maps API cost reduction
├── cs-insurance-claims.html     # PM case study: insurance claims 0-to-1
├── gitra-project.html           # Design case study: Gitra
├── jurnal-kamu-project.html     # Design case study: JurnalKamu
├── helm-in-project.html         # Design case study: Helm-In
├── css/
│   ├── main.css                 # Global design system
│   └── case-study.css           # Shared layout for PM case studies
├── img/
│   ├── Header.png
│   ├── JurnalKamuThumbnail.png
│   ├── pas foto.png
│   └── portfolioThumb1.png
├── case-study-dbs-integration.md        # Source narrative (not published)
├── case-study-google-maps-api.md        # Source narrative (not published)
├── case-study-insurance-claims.md       # Source narrative (not published)
├── README.md
└── DEVLOG.md
```

**Note:** The `case-study-*.md` files are source content. They are not linked or published. They were used to write the HTML case study pages.

---

## Key Patterns Reference

### Case study page template structure
```html
<body style="--page-accent: #COLOR;">
    <!-- NAV -->
    <nav class="cs-nav">
        <a href="index.html" class="cs-nav__logo">R<span>.</span>Z</a>
        <a href="portfolio.html" class="cs-nav__back">← All work</a>
    </nav>

    <!-- HERO -->
    <section class="cs-hero">
        <div class="cs-hero__inner">
            <div class="cs-hero__eyebrow">COMPANY · CATEGORY · YEAR</div>
            <h1 class="cs-hero__title">Title<br><em>Emphasis</em></h1>
            <div class="cs-meta">
                <div class="cs-meta__cell">...</div> <!-- ×4 -->
            </div>
        </div>
    </section>

    <!-- BODY SECTIONS -->
    <div class="cs-body">
        <section class="cs-section">
            <div class="cs-section__label">01 / Section Name</div>
            <h2 class="cs-h2">Heading</h2>
            <p class="cs-text">Body paragraph</p>
            <div class="cs-diagram">...</div>
        </section>
    </div>

    <!-- IMPACT -->
    <section class="cs-impact">
        <div class="cs-impact__inner">
            <div class="cs-impact__grid cols-4">
                <div class="cs-impact__item">
                    <span class="cs-impact__value">VALUE</span>
                    <p class="cs-impact__desc">Description</p>
                </div>
            </div>
        </div>
    </section>

    <!-- TAKEAWAY -->
    <section class="cs-takeaway">
        <div class="cs-takeaway__inner">
            <p class="cs-takeaway__text">Closing reflection.</p>
        </div>
    </section>

    <!-- FOOTER -->
    <footer class="cs-footer">
        <div class="cs-footer__inner">
            <a href="PREV.html" class="cs-footer__link">← Previous</a>
            <a href="portfolio.html" class="cs-footer__back-to">All work</a>
            <a href="NEXT.html" class="cs-footer__link">Next →</a>
        </div>
    </footer>
</body>
```

### Animated bar chart pattern
```css
/* In page <style> block */
.bar { width: 0; transition: width 0.8s cubic-bezier(0.4, 0, 0.2, 1); }
.chart.animated .bar { width: var(--w); }

@media (prefers-reduced-motion: reduce) {
    .bar { width: var(--w); transition: none; }
}
```
```js
const obs = new IntersectionObserver((entries) => {
    entries.forEach(e => {
        if (e.isIntersecting) {
            e.target.classList.add('animated');
            obs.unobserve(e.target);
        }
    });
}, { threshold: 0.3 });
obs.observe(document.getElementById('chartId'));
```

### Scroll reveal (shared, from main.css / case-study.css)
```css
.reveal { opacity: 0; transform: translateY(20px);
    transition: opacity 0.65s cubic-bezier(0.4, 0, 0.2, 1),
                transform 0.65s cubic-bezier(0.4, 0, 0.2, 1); }
.reveal.visible { opacity: 1; transform: translateY(0); }
.reveal-delay-1 { transition-delay: 0.1s; }
.reveal-delay-2 { transition-delay: 0.2s; }
.reveal-delay-3 { transition-delay: 0.3s; }
```
```js
const obs = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('visible');
            obs.unobserve(entry.target);
        }
    });
}, { threshold: 0.12, rootMargin: '0px 0px -40px 0px' });
document.querySelectorAll('.reveal').forEach(el => obs.observe(el));
```

### Nav scrolled effect (case study pages)
```js
window.addEventListener('scroll', () => {
    nav.classList.toggle('cs-nav--scrolled', window.scrollY > 20);
}, { passive: true });
```

---

# Session 3 — Restructuring Landing Page

## Reorganizing `index.html` Core Layout

### Problem
The landing page got too vertically deep and blended conceptual context (Approach, Impact) with the core portfolio (Work) and background (About). The visual rhythm felt disjointed.

### Decision
Simplifying `index.html` strictly to its core funnel: Who I am (Hero, About), what I've done (Case Studies), and how to reach me (Contact). The context-heavy segments (Approach, Impact) belong naturally inside the CV/About page. The sparse 'Writing' section was removed entirely until it warrants a full standalone presence.

**Structural Update:**
- **Hero**
- **01 / About** (Moved up, right below Hero)
- **02 / Case Studies** (Re-numbered)
- **03 / Contact** (Re-numbered, removed 'Writing' above it)

Nav links in `index.html` were reduced from 5 to 3: **About · Work · Contact**

## Transplanting into `about.html`

- The `Approach` section was moved to immediately follow the bio block in `about.html`. It works logically as part of the broader "Product thinker / Technical builder" narrative.
- The `Impact` metric strip was moved out of the homepage and embedded directly after `Approach`, right before the timeline of `Experience`. The label was appropriately set to "Impacts".
