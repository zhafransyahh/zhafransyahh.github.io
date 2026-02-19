# Rafi Zhafransyah — Personal Portfolio

Personal portfolio and CV site for Muhammad Rafi Zhafransyah, Technical Product Manager based in Jakarta.

**Live site:** [zhafransyahh.github.io](https://zhafransyahh.github.io)

---

## Purpose

A unified personal site that integrates professional CV data (work experience, skills, education) with detailed product case studies from Apple Developer Academy projects. Designed to serve as both a portfolio showcase and a professional profile.

## Design Decisions

**Direction:** Dark editorial — near-black background (#0D0D0C), warm off-white type (#F0EDE6), terracotta accent (#C75B39). The aesthetic is serious, premium, and intentionally non-generic.

**Typography:**
- **Instrument Serif** — Display/headings. Elegant with strong character at large sizes.
- **Space Mono** — Body, labels, metadata. Provides technical credibility and clear hierarchy contrast.

**Layout principles:**
- Content-first: no decorative elements that don't serve information hierarchy
- Section labels use numbered prefixes (01 / About, 02 / Experience) for editorial structure
- Timeline-based experience section with quantified metric callouts
- Portfolio items use side-by-side thumbnail + metadata layout for scanability
- Skills split into two tiers: Product Skills (list) and Technical Tools (chip grid)

**Case studies** retain their original unique color palettes (amber for Gitra, plum for JurnalKamu, blue for Helm-In) as self-contained editorial pieces, with a consistent back-navigation bar linking to the main site.

## Tech Stack

- Vanilla HTML5, CSS3, JavaScript
- Google Fonts (Instrument Serif, Space Mono)
- No frameworks, no build tools, no dependencies
- IntersectionObserver for scroll-reveal animations
- CSS custom properties for design tokens
- Mobile-first responsive design
- `prefers-reduced-motion` support

## File Structure

```
/
├── index.html                 # Homepage (hero, about, experience, portfolio, skills, education, contact)
├── about.html                 # Extended about page with full CV details
├── portfolio.html             # Portfolio listing page
├── gitra-project.html         # Case study: Gitra guitar learning app
├── jurnal-kamu-project.html   # Case study: JurnalKamu mental health journal
├── helm-in-project.html       # Case study: Helm-In smart helmet rental
├── css/
│   └── main.css               # Design system stylesheet
└── img/
    ├── Header.png             # Gitra project header
    ├── JurnalKamuThumbnail.png# JurnalKamu thumbnail
    ├── pas foto.png      # Profile photo
    └── portfolioThumb1.png    # Helm-In thumbnail
```

## How to Update Content

**To update CV/work experience:** Edit the timeline section in `index.html` and `about.html`. Each role is an `<article class="timeline__item">` block. Metrics are in `.timeline__metrics` containers.

**To add a new project:** Duplicate one of the existing project HTML files, update the content and color palette, then add a new `.work__item` link in both `index.html` and `portfolio.html`.

**To change colors:** Modify the CSS custom properties in `:root` at the top of `css/main.css`. The accent color (`--accent`) is the primary brand color used for highlights, links, and interactive states.

**To update personal info:** Contact details (email, social links) appear in the contact section of every page — update in `index.html`, `about.html`, and `portfolio.html`.

## About

Muhammad Rafi Zhafransyah is a Technical Product Manager at JULO, one of Indonesia's leading fintech lending platforms, with 4+ years of experience owning end-to-end product delivery across Channeling & Funding, Onboarding, and Underwriting. Previously at Apple Developer Academy Indonesia.
