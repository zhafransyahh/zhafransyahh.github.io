# Portfolio Dev Log — zhafransyahh.github.io

A full record of design decisions, technical changes, errors encountered, and the reasoning behind them. Covers the entire build session for Muhammad Rafi Zhafransyah's personal portfolio.

---

## Stack

- **Vanilla HTML5 / CSS3 / JS** — no frameworks, no build tools
- **Google Fonts**: Fraunces (serif headlines), DM Sans (body), Space Mono (labels, nav, tags)
- **Hosting**: GitHub Pages from `master` branch
- **Repo**: `zhafransyahh/zhafransyahh.github.io`
- **Pages**: `index.html`, `gitra-project.html`, `jurnal-kamu-project.html`, `helm-in-project.html`, `404.html`

---

## Design System

Defined in `css/main.css` as CSS custom properties:

```css
:root {
    --accent: #C75B39;         /* warm terracotta — primary brand color */
    --accent-hover: #D4694A;
    --bg: #0D0D0C;             /* near-black background */
    --bg-subtle: #161614;
    --bg-card: #1A1A18;
    --text-primary: #F0EDE6;   /* warm off-white */
    --text-secondary: #9C978E;
    --text-muted: #5E5B54;
    --border: #2A2825;
    --border-light: #363330;
    --serif: 'Fraunces', Georgia, serif;
    --sans: 'DM Sans', system-ui, sans-serif;
    --mono: 'Space Mono', 'Courier New', monospace;
}
```

**Theme**: Dark editorial. High contrast, warm neutrals, serif headlines, monospace for metadata/labels.

---

## Typography Decision

### Problem
Original font stack used **Instrument Serif + Space Mono**. Space Mono was the body font — this was hard to read at paragraph length due to its fixed-width letterforms and tight tracking.

### Options considered
- **Option A**: Keep Instrument Serif, replace body with Inter — safe but generic
- **Option B**: Switch to Playfair Display + Lato — classic editorial, less distinctive
- **Option C**: Fraunces + DM Sans + keep Space Mono for labels only

### Decision: Option C
- **Fraunces** — optical-size aware serif, has a natural italic, distinctive but not novelty. Used only for headlines (`h1`, `h2`, role titles).
- **DM Sans** — proportional, humanist sans-serif. High readability at body sizes. Replaced Space Mono as the body font.
- **Space Mono** — kept strictly for nav links, section labels, tags, timestamps. Its rigidity works well for small metadata, not for paragraphs.

### Implementation
```css
body { font-family: var(--sans); font-size: 0.9375rem; } /* was Space Mono at 0.875rem */
.hero__label, .nav__links a, .timeline__company, .timeline__date { font-family: var(--mono); }
.hero__name, .about__heading, .timeline__role { font-family: var(--serif); }
```

Google Fonts link updated across all pages to load Fraunces + DM Sans + Space Mono.

---

## Hero Section

### Profile photo — tried in hero, reverted to About
Moved the profile photo into the hero section as a two-column grid (text left, photo right). **Didn't look good** — disrupted the editorial single-column feel. Reverted photo back to the About section.

Settings kept from the hero experiment:
- Size: 380px wide (was 280px)
- `aspect-ratio: 3/4` (portrait orientation)
- `border-radius: 16px` (softer than the original sharp corners)
- `filter: grayscale(20%) brightness(1.05)` — slightly desaturated and brightened for a refined editorial tone
- `object-position: center top` — keeps face in frame

### Scroll indicator animation removed
The `.hero__scroll` element had a pulse/sway keyframe animation on its `::after` line. Removed the animation (`animation: none`, then simplified to static `opacity: 0.5`). Reason: animation was distracting and served no functional purpose.

On mobile: the scroll indicator is hidden entirely (`display: none`) since swipe gesture replaces it.

---

## Experience Section

### Initial state
Generic bullet list with no visual hierarchy.

### Readability improvements
1. **Metric cards** — key numbers displayed in a card grid (`display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr))`). Value in Fraunces serif at 2rem (accent color), label in DM Sans at 0.75rem (text-secondary).
2. **Company name** — switched to Space Mono, `0.6875rem`, text-secondary. Creates clear subordinate hierarchy below the role title.
3. **Bullet points** — size increased from `0.8125rem` to `0.9375rem` (matched body text). Color: `text-secondary`. Custom dot marker in accent color.
4. **Item spacing** — `padding-bottom` increased from `3rem` to `4rem` per timeline item for breathing room.

### Full LinkedIn experience added
6 roles added across 4 companies:
1. JULO — PM Channeling (current)
2. JULO — PM Onboarding
3. JULO — Associate PM
4. Apple Developer Academy — grouped with multiple sub-roles
5. Deaf to Dev — volunteer
6. Binus University — marketing support (part-time)

Deaf to Dev and Binus were deemed less relevant to the PM/product focus of the site. Hidden behind a **"Show earlier roles"** toggle (see below).

---

## Show Earlier Roles Toggle

### Purpose
Deaf to Dev and Binus University roles are real experience but not the focus of a PM portfolio. They're preserved (not deleted) but collapsed behind a toggle so recruiters don't skip past the important roles.

### First attempt — `hidden` attribute
```js
moreSection.hidden = !moreSection.hidden;
```
**Problem**: CSS transitions (`max-height`, `opacity`) don't animate when `hidden` is toggled because `display: none` is applied instantly. The expand/collapse had no animation.

### Fix — CSS `max-height` + `opacity` transition
```css
.timeline__more {
    max-height: 0;
    overflow: hidden;
    opacity: 0;
    transition: max-height 0.5s cubic-bezier(0.4, 0, 0.2, 1), opacity 0.4s ease;
}
.timeline__more.is-open {
    max-height: 800px;
    opacity: 1;
}
```
JS updated to toggle class instead of attribute:
```js
moreSection.classList.toggle('is-open');
```
On init: `moreSection.removeAttribute('hidden')` to ensure CSS controls visibility, not the attribute.

### Toggle button alignment — multi-attempt fix

**Structure:**
```
.timeline                    ← padding-left: 2rem
  .timeline::before          ← vertical line at left: 5px
  .timeline__item
    .timeline__marker        ← left: -2rem (at the padding boundary)
    .timeline__content       ← padding-left: 1rem
      [text content]         ← starts at 2rem + 1rem = 3rem from section edge
  .timeline__toggle          ← direct child of .timeline
```

**Attempts:**
- `margin-left: 1rem` → too far right (was aligning to 2rem + 1rem = 3rem, but visually off)
- `margin-left: 3rem` → way too far right
- `margin-left: 0` → aligned to the vertical line, not the text
- **Correct**: `margin-left: 1rem` → puts button at `2rem (timeline padding) + 1rem = 3rem` from section edge, matching text content start ✓

**Gap issue**: When the hidden section opened, there was a large gap before the first item. Cause: `.timeline__item:last-child { padding-bottom: 0 }` only applied within `.timeline`, not inside `.timeline__more`. Fixed by:
```css
.timeline__more .timeline__item:first-child { padding-top: 2rem; }
.timeline__more .timeline__item:last-child { padding-bottom: 0; }
```

Also removed an inline `style="padding-bottom:0;"` that was left as a stale hack from an earlier approach.

---

## Testimonials Section

4 LinkedIn recommendations added between Education and Contact. Grid layout: 1 column mobile, 2 columns at 768px+.

```html
<blockquote class="testimonial__card">
    <p class="testimonial__quote">...</p>
    <footer class="testimonial__author">
        <p class="testimonial__name">Name</p>
        <p class="testimonial__title">Title · Company</p>
    </footer>
</blockquote>
```

Nav updated: added "Testimonials" link between Skills and Contact. Section numbered 06, Contact renumbered to 07.

---

## Contact Nav Highlight Bug

### Problem
The Contact section (`#contact`) is at the very bottom of the page. The IntersectionObserver used for active nav highlighting had `rootMargin: '-80px 0px -40% 0px'` — this clipped 40% from the bottom of the viewport, which was often larger than the Contact section itself. The observer never fired.

### Fix
Added a scroll event listener alongside the observer:
```js
window.addEventListener('scroll', () => {
    const atBottom = window.innerHeight + window.scrollY >= document.body.scrollHeight - 10;
    if (atBottom) setActive('contact');
});
```

Also added immediate click activation — clicking a nav link sets it active instantly without waiting for the scroll to settle:
```js
navLinks.forEach(link => {
    link.addEventListener('click', () => {
        navLinks.forEach(l => l.classList.remove('active'));
        link.classList.add('active');
    });
});
```

---

## Case Study Pages — Text Overflow Fix

### Problem
Text in case study sections (gitra, jurnal-kamu, helm-in) was stretching to the full viewport width. On wide monitors, a single paragraph could span 1400px+ — completely unreadable.

### Root cause
```css
section { padding: 6rem 4rem; }
/* No max-width container on the section content */
```

### Fix
```css
section { padding: 6rem 2rem; }
section > * {
    max-width: 1100px;
    margin-left: auto;
    margin-right: auto;
}
```
The `section > *` selector constrains all direct children without clipping full-bleed backgrounds, which remain on the `section` itself.

---

## Case Study Hero Restructure

### Problem
Each case study hero had an inner `<nav class="hero-nav">` with the project name logo on the left and "UX / UI Case Study" tag on the right — both in one row. This looked like a squashed nav bar, not a proper hero.

### Before
```html
<nav class="hero-nav">
    <span class="logo">Gitra</span>
    <span class="tag">UX / UI Case Study</span>
</nav>
<div class="hero-content">
    <p class="hero-eyebrow">Mobile App Design · Guitar Learning</p>
    <h1 class="hero-title">Everyone can learn guitar.</h1>
</div>
```

### After
```html
<div class="hero-content">
    <p class="hero-eyebrow">UX / UI Case Study</p>
    <h1 class="hero-title"><em>Gitra</em></h1>
    <p class="hero-desc">...</p>
</div>
```

- Removed the inner `hero-nav` entirely from all 3 case study pages
- Moved the case study type label into `hero-eyebrow` (small monospace label above the title)
- Project name promoted to the main `hero-title` (large serif)
- Grid changed from `auto 1fr auto` (3 rows: nav, content, meta) to `1fr auto` (2 rows: content, meta)
- `hero-content` changed to `justify-content: flex-end` so content sits above the meta bar
- Removed all unused `.hero-nav`, `.hero-nav .logo`, `.hero-nav .tag` CSS

Applied to: gitra, jurnal-kamu, helm-in.

---

## Hero Top Margin — Case Study Pages

After the hero restructure, the hero content was sitting behind the fixed nav bar (44px tall). Fixed by increasing hero top padding:

- Desktop: `4.5rem 4rem 3rem` (unchanged)
- Mobile: `3.5rem 1.5rem 2rem` → `4rem 1.5rem 2rem`

---

## Git Setup & Push Issues

### Initial push attempt
Tried pushing to GitHub — branch was named `main` locally but GitHub Pages for this repo serves from `master`.

### Fix
```bash
git branch -m main master           # rename local branch
git push origin master --force      # push to correct remote branch
git push origin --delete main       # remove the wrong branch
```

### Authentication
Repo required a personal access token (PAT) for HTTPS auth. Token is scoped to the specific repo. **Note: the token shared during the session should be regenerated** — it was exposed in the chat.

---

## Mobile Optimization — iOS & Android

Full audit and optimization pass. Changes applied across all 5 HTML files and `css/main.css`.

### Meta tags added to all pages

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="theme-color" content="#0D0D0C">  <!-- matches --bg on index.html -->
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
```

Each case study page gets its own `theme-color` matching its hero background:
- Gitra: `#1C1C1E` (charcoal)
- Jurnal Kamu: `#521F3F` (deep plum)
- Helm-In: `#0A0F1E` (dark navy)

### iOS-specific fixes

| Issue | Fix |
|-------|-----|
| `100vh` bug — hero extends behind browser chrome | `@supports (min-height: 100dvh) { .hero { min-height: 100dvh; } }` |
| Content behind notch (Dynamic Island, landscape) | `env(safe-area-inset-*)` on `.nav` padding and `.hero` padding-top |
| iOS auto-zooms on sub-16px text | `-webkit-text-size-adjust: 100%; text-size-adjust: 100%` on body |
| Blue tap flash on every touch | `-webkit-tap-highlight-color: transparent` on body |
| 300ms tap delay | `touch-action: manipulation` on global `*` reset |
| Jurnal Kamu user flow — no momentum scroll | `-webkit-overflow-scrolling: touch` on the horizontal scroll container |

### Android-specific fixes

| Issue | Fix |
|-------|-----|
| Pull-to-refresh interrupts navigation | `overscroll-behavior-y: none` on body |
| Hover states stick after tap | `@media (hover: hover)` wraps work card hover effects |
| No touch feedback on work cards | `@media (hover: none) { .work__item:active { background: ... } }` |
| Jank on work thumbnail scale animation | `will-change: transform` on `.work__thumb` |
| Android status bar color | `meta theme-color` per page |

### Layout fixes (both platforms)

| Issue | Fix |
|-------|-----|
| Nav toggle 28×20px — too small to tap | Expanded to 44×44px hit area in `@media (max-width: 768px)`, visual unchanged |
| Social icon links 20×20px — too small | `min-width: 44px; min-height: 44px` on `.contact__socials a` |
| Section padding 7rem = 112px — excessive on mobile | Reduced to `4rem` in `@media (max-width: 768px)` |
| Timeline metrics grid overflows 335px content width | `grid-template-columns: 1fr` on mobile |
| Hero meta items — 3rem gap causes wide spreading | Reduced to `1.5rem` in case study mobile media queries |
| Hero "SCROLL" indicator clutters small screens | `display: none` in `@media (max-width: 768px)` |
| Footer elements left-aligned when stacked | `flex-direction: column; align-items: center; text-align: center` |
| Hero name "Zhafransyah" too large on 375px screens | `clamp(2.25rem, 9vw, 3rem)` in `@media (max-width: 480px)` |
| Container too wide for edge-to-edge content | `padding: 0 1.25rem` (from 0 2rem) — gives 24px more content width |

---

## Errors Encountered

### Edit tool "file not read" error
**Cause**: Attempted to use Edit tool on a file without reading it first in the same session.
**Fix**: Always read the file with the Read tool before making any edits. The Edit tool requires the file content to be in context to do exact string matching.

### Toggle animation not working with `hidden` attribute
**Cause**: CSS `max-height` and `opacity` transitions require the element to be `display: block` (or similar) at all times. The `hidden` HTML attribute applies `display: none` which bypasses transitions entirely.
**Fix**: Removed the `hidden` attribute. Used CSS-only show/hide via `max-height: 0` → `max-height: 800px` + `opacity` transition controlled by an `.is-open` class.

### Push to wrong branch (`main` vs `master`)
**Cause**: `git init` defaults to `main` on newer Git versions, but the GitHub Pages source was set to `master`.
**Fix**: Renamed branch and force-pushed to master, then deleted the remote `main` branch.

### Contact nav highlight never firing
**Cause**: IntersectionObserver with `rootMargin: '-80px 0px -40% 0px'` clips 40% from the viewport bottom. The Contact section is short and sits at the very bottom of the page — the observer's intersection never triggered because the section was smaller than the clipped area.
**Fix**: Supplementary scroll listener checking `window.innerHeight + window.scrollY >= document.body.scrollHeight - 10`.

### 100vh iOS bug
**Cause**: On iOS Safari, `100vh` is calculated including the browser chrome (address bar, tab bar). When the page first loads and the address bar is visible, the hero section is taller than the visible viewport.
**Fix**: `100dvh` (dynamic viewport height) — recalculates as browser chrome shows/hides. Wrapped in `@supports` for safe progressive enhancement.

---

## File Structure

```
zhafransyahh.github.io-master/
├── index.html                    # Main portfolio page
├── gitra-project.html            # Guitar learning app case study
├── jurnal-kamu-project.html      # Mental health journal case study
├── helm-in-project.html          # Helmet rental app case study
├── 404.html                      # Custom 404 page
├── about.html                    # (exists, separate about page)
├── portfolio.html                # (exists, portfolio listing page)
├── css/
│   └── main.css                  # All styles for index.html
├── img/
│   ├── pas foto.png              # Profile photo (compressed)
│   ├── Header.png                # Gitra thumbnail
│   ├── JurnalKamuThumbnail.png   # Jurnal Kamu thumbnail
│   └── portfolioThumb1.png       # Helm-In thumbnail
└── DEVLOG.md                     # This file
```

**Note**: Case study pages use inline `<style>` blocks — they are self-contained and do not import `css/main.css`. They have their own design tokens and typography.

---

## Key CSS Patterns

### Timeline vertical line + marker
```css
.timeline { position: relative; padding-left: 2rem; }
.timeline::before {
    content: ''; position: absolute;
    left: 5px; top: 0; bottom: 0;
    width: 1px; background: var(--border);
}
.timeline__marker {
    position: absolute; left: -2rem; top: 6px;
    width: 11px; height: 11px;
    border-radius: 50%; background: var(--accent);
}
.timeline__content { padding-left: 1rem; }
/* Content text = 2rem (timeline padding) + 1rem (content padding) = 3rem from section edge */
/* Toggle button = margin-left: 1rem → 2rem + 1rem = 3rem ✓ aligned */
```

### Max-width content inside full-bleed sections (case studies)
```css
section { padding: 6rem 2rem; }
section > * { max-width: 1100px; margin-left: auto; margin-right: auto; }
```

### Smooth show/hide toggle (CSS transition, not display:none)
```css
.timeline__more {
    max-height: 0; overflow: hidden; opacity: 0;
    transition: max-height 0.5s cubic-bezier(0.4, 0, 0.2, 1), opacity 0.4s ease;
}
.timeline__more.is-open { max-height: 800px; opacity: 1; }
```

### Active nav highlight with scroll-to-bottom detection
```js
// IntersectionObserver for most sections
// + supplementary scroll check for Contact (always at bottom)
window.addEventListener('scroll', () => {
    if (window.innerHeight + window.scrollY >= document.body.scrollHeight - 10) {
        setActive('contact');
    }
});
```

### Mobile-safe hover effects
```css
@media (hover: hover) {
    /* Only applies on devices with a real pointer — not touch */
    .work__item:hover .work__thumb { transform: scale(1.02); }
}
@media (hover: none) {
    /* Touch devices get :active feedback instead */
    .work__item:active { background: rgba(199, 91, 57, 0.06); }
}
```

### iOS safe-area insets
```css
.nav {
    padding-left: max(2rem, env(safe-area-inset-left));
    padding-right: max(2rem, env(safe-area-inset-right));
}
.hero {
    padding-top: max(5rem, calc(env(safe-area-inset-top) + 4rem));
}
```

---

## Commit History (this session)

```
138a875  Replace profile photo with compressed version for faster load
d0334d7  Optimize for iOS and Android mobile
195ab73  Restructure hero on all case study pages: label on top, title below
4982b43  Fix toggle button alignment and remove stale inline style
9184ac5  Fix earlier roles toggle alignment and spacing
dae04ae  Fix hero top margin on all case study pages
...      (earlier commits: fonts, photo, experience, testimonials, nav, contact)
```
