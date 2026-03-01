# Rafi Zhafransyah | Personal Portfolio

Personal portfolio and CV site for Muhammad Rafi Zhafransyah, Technical Product Manager based in Jakarta.

**Live site:** [zhafransyahh.github.io](https://zhafransyahh.github.io)

---

## Purpose

A unified personal site that combines professional PM case studies (from JULO) with a CV and Academy design work (Apple Developer Academy). Designed to serve recruiters across industries — not just fintech — by framing work around transferable PM skills: enterprise integrations, cost optimization, 0-to-1 builds.

---

## Design System

Defined in `css/main.css` as CSS custom properties:

```css
:root {
    --accent: #808CB2;         /* Slate Blue — primary brand color */
    --bg: #0c1220;             /* near-black background */
    --bg-subtle: #111827;
    --bg-card: #151d2e;
    --text-primary: #d1d5db;
    --text-secondary: #8892a4;
    --text-muted: #4a5568;
    --border: rgba(228,232,237,0.08);
    --serif: 'Fraunces', Georgia, serif;
    --sans: 'DM Sans', system-ui, sans-serif;
    --mono: 'Space Mono', 'Courier New', monospace;
}
```

**Theme:** Dark editorial. Navy-black background, cool off-white type, Slate Blue accent. Serious, premium, intentionally non-generic.

**Typography:**
- **Fraunces** — Optical-size aware serif. Used for headlines, hero titles, impact values.
- **DM Sans** — Humanist sans-serif. Body text, descriptions, card content.
- **Space Mono** — Fixed-width. Nav, labels, tags, metadata, diagram captions.

**Global font scale:** `html { font-size: 24px }` — all rem units scale at 1.5x the browser default, producing a "150% zoom" density at native zoom. This is intentional.

---

## Per-Page Accent Colors (Case Studies)

Each PM case study has a scoped accent via `--page-accent` on the body:

| Page | Accent | Hex |
|---|---|---|
| `cs-lender-integration.html` | Slate Blue | `#808CB2` |
| `cs-maps-cost.html` | Teal | `#3d8c6e` |
| `cs-insurance-claims.html` | Indigo | `#7b5ea7` |

Legacy Academy case studies (Gitra, JurnalKamu, Helm-In) retain their original self-contained color palettes.

---

## File Structure

```
/
├── index.html                   # Homepage: hero, about, work, contact
├── about.html                   # Extended about / CV page (includes approach, impact)
├── portfolio.html               # Portfolio listing: JULO case studies + Gitra
├── 404.html                     # Custom 404 page
│
├── css/
│   ├── main.css                 # Global design system (tokens, layout, nav, components)
│   └── case-study.css           # Shared layout system for the 3 JULO case study pages
│
├── cs-lender-integration.html      # Case study: "No Map of the Territory" (institutional lender integration)
├── cs-maps-cost.html            # Case study: "Invisible at Low Volume" (Maps API cost reduction)
├── cs-insurance-claims.html     # Case study: "The Arithmetic of Trust" (insurance claims 0-to-1)
│
├── gitra-project.html           # Case study: "What the Hands Already Know" (Gitra, Apple Dev Academy)
├── jurnal-kamu-project.html     # Case study: JurnalKamu mental health journal
├── helm-in-project.html         # Case study: Helm-In smart helmet rental
│
├── img/
│   ├── Header.png               # Gitra project header
│   ├── JurnalKamuThumbnail.png  # JurnalKamu thumbnail
│   ├── pas foto.png             # Profile photo
│   └── portfolioThumb1.png      # Helm-In thumbnail
│
├── case-study-dbs-integration.md        # Source narrative for DBS case study
├── case-study-google-maps-api.md        # Source narrative for Maps cost case study
├── case-study-insurance-claims.md       # Source narrative for insurance claims case study
│
├── README.md
└── DEVLOG.md
```

---

## Page Navigation Structure

### `index.html` (homepage)
Sections with anchor IDs:
- `#about` — 01 / About
- `#work` — 02 / Case Studies (JULO + Gitra)
- `#contact` — 03 / Contact

Nav links: About · Work · Contact

### `portfolio.html`
- Professional Work: JULO (2024) — 3 case study cards
- Academy Work: Apple Developer Academy — Gitra (featured)

### Case study pages (`cs-*.html`)
Use `css/case-study.css` for shared layout. Nav: R.Z logo (links `index.html`) + "← All work" (links `portfolio.html`).

Footer navigation: ← Previous case study · → Next case study

---

## CSS Architecture

Two stylesheet layers:

**`css/main.css`** — everything for the main site:
- Design tokens (`:root` CSS custom properties)
- Reset and base
- Nav (`.nav`, `.nav__toggle`, mobile hamburger)
- Page header (`.page-header`)
- Section and container
- Hero
- Work grid (`.work__item`, `.work__thumb`, `.work__tag`)
- About, philosophy, impact, contact, footer
- Writing section
- Scroll reveal (`.reveal`, `.reveal-delay-*`)
- Mobile breakpoints

**`css/case-study.css`** — shared layout for PM case study pages:
- `.cs-nav` — case study nav (scrolled glassmorphism effect)
- `.cs-hero` — full-height hero with metadata strip
- `.cs-meta` — 4-cell metadata grid (Role / Timeline / Company / Impact)
- `.cs-body` — prose content wrapper
- `.cs-section` — content sections with border-bottom dividers
- `.cs-h2`, `.cs-text`, `.cs-pullquote`, `.cs-list` — typography components
- `.cs-diagram` — diagram container
- `.cs-impact` — impact metrics strip
- `.cs-takeaway` — closing quote section
- `.cs-footer` — prev/next navigation

Page-specific styles (diagrams, charts) are in `<style>` blocks inline on each page.

---

## Case Study Titles

The four main case studies use evocative, literary titles inspired by the style of Dario Amodei's "Machines of Loving Grace":

| Title | Subject | File |
|---|---|---|
| No Map of the Territory | Institutional lender integration, inherited mid-project | `cs-lender-integration.html` |
| Invisible at Low Volume | Maps API 99% cost reduction via UX fix | `cs-maps-cost.html` |
| The Arithmetic of Trust | Insurance claims system, 0-to-1 | `cs-insurance-claims.html` |
| What the Hands Already Know | Gitra accessible guitar learning app | `gitra-project.html` |

---

## How to Update Content

**Add a new PM case study:**
1. Copy `cs-maps-cost.html` as a template (most standard structure)
2. Set `--page-accent` on `<body>` with a new color
3. Link `css/main.css` and `css/case-study.css`
4. Add a `.work__item` card to both `index.html` (#work section) and `portfolio.html`

**Update CV / work experience:** Edit `about.html` timeline section. Each role is `<article class="timeline__item">`.

**Change accent color:** Modify `--accent` in `css/main.css` `:root`. Case study accents are scoped per-page via `--page-accent`.

**Update contact details:** Email and social links appear in the contact section of `index.html`, `about.html`, and `portfolio.html` — update in all three.

**Change global scale:** `html { font-size }` in `css/main.css`. Currently `24px` (1.5x default). Set to `16px` for standard browser scale.

---

## About

Muhammad Rafi Zhafransyah is a Technical Product Manager at JULO with 4+ years owning end-to-end product delivery across Channeling & Funding, Onboarding, and Underwriting. Previously at Apple Developer Academy Indonesia. Portfolio is intentionally framed for any industry — the work is about systems, integrations, and precision, not about fintech specifically.
