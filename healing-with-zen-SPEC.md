# Healing with Zen — Website Rebuild Spec

**Deliverable:** An awwwards-caliber, single-page marketing site that replaces the current cluttered WordPress/Elementor site at `healingwithzen.com`. This is a **client presentation demo**, deployed to **GitHub Pages**.

**Author's note to the builder (Claude Code):** Everything you need is in this file — real content, the design system, section-by-section layout, and animation specs. Follow the design tokens exactly. Do not substitute a generic template. The bar is: _this could win a site-of-the-day._ Build to a quality floor (responsive to mobile, visible keyboard focus, `prefers-reduced-motion` respected) without cutting the signature moment.

---

## 1. Product & audience

- **Subject:** Healing with Zen — an acupuncture & Traditional Chinese Medicine (TCM) clinic in Pasadena, CA. Founded 2013. Practitioners: **Zen Tuan** (Acupuncturist, Herbalist, Clinical Director) and **Kiera Layne** (Acupuncturist, Herbalist).
- **Audience:** Wellness-conscious Pasadena/San Gabriel Valley adults (skews 30–60, women's-health-heavy) seeking natural relief for pain, stress, fertility, digestion, and chronic conditions. They value calm, credibility, and a premium feel.
- **The page's single job:** get the visitor to **book a session**. Every section funnels to the booking CTA.
- **Primary CTA target (real):** `https://healingwithzen.janeapp.com/`

---

## 2. Tech stack & hard constraints

- **Static site, no build step.** Must run by opening `index.html` and deploy to GitHub Pages as-is.
- **Single self-contained `index.html`** with inline `<style>` and `<script>` is acceptable and preferred for demo simplicity. (If you prefer split files, keep them relative: `/index.html`, `/assets/css/styles.css`, `/assets/js/main.js`. No bundler.)
- **Libraries via CDN only:**
  - GSAP 3 core + ScrollTrigger — `https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js` and `.../ScrollTrigger.min.js`
  - Lenis smooth scroll — `https://cdn.jsdelivr.net/npm/@studio-freight/lenis@1.0.42/dist/lenis.min.js` (wire Lenis's `scroll` event into `ScrollTrigger.update`, and drive Lenis from `gsap.ticker`).
  - Google Fonts (see §4).
- **No images required.** Do **not** hotlink the old site's photos (they hotlink-block and are low-res). Build all visuals as **inline SVG / CSS**. Leave clearly-labeled `<!-- PHOTO SLOT -->` comments where real photography drops in later (hero portrait, practitioner headshots, service cards).
- **Vanilla JS only** (no React/Vue). Split text for animation with a small custom line/word splitter — do **not** rely on GSAP's SplitText plugin.
- Target: Lighthouse Performance ≥ 90, no console errors, works offline once loaded (except fonts/CDNs).

---

## 3. The signature concept — "The Meridian"

Reject the generic wellness look (cream + serif + terracotta). Ground the design in the actual world of Chinese medicine: **meridians, qi flow, the needle's path, herbs.**

**Signature element:** a single continuous **qi-line** — a thin meandering SVG path fixed behind the content — that **draws itself top-to-bottom as the user scrolls** (animate `stroke-dashoffset` with a ScrollTrigger `scrub` tied to full-page progress). The site's spine _is_ a meridian.

- Along the line, place **acupuncture-point nodes** (small circles) positioned near each section anchor. As each section enters, its node **pulses / fills** and the connecting line segment lights up in jade → moxa gold.
- **Section markers use real acupuncture point codes** instead of generic `01 / 02 / 03`. This encodes something true about the subject. Map:
  | Section | Point code (eyebrow marker) |
  |---|---|
  | Hero | `DU-20` (Baihui — "hundred meetings") |
  | Trust / stats | `ST-36` (Zusanli — vitality) |
  | What we heal | `LI-4` (Hegu — the great eliminator) |
  | How we heal | `REN-6` (Qihai — "sea of qi") |
  | Services | `SP-6` (Sanyinjiao) |
  | Practitioners | `HT-7` (Shenmen — "spirit gate") |
  | Testimonials | `KI-3` (Taixi) |
  | Book / CTA | `LR-3` (Taichong) |
  | Contact / footer | `GB-20` (Fengchi) |

Use these as the small mono eyebrow label above each section heading, e.g. `LI-4 · WHAT WE HEAL`.

---

## 4. Design system (tokens — use verbatim)

### Color

```css
:root {
  --paper: #eae4d5; /* warm rice-paper base (light sections) */
  --paper-2: #e1d9c6; /* slightly deeper paper for cards/insets */
  --ink: #1e2a24; /* deep pine ink, near-black green (dark sections, text) */
  --ink-soft: #2c3a32; /* elevated dark surface */
  --jade: #3e6b55; /* brand jade — primary green */
  --jade-2: #557e63; /* lighter jade for hovers/links */
  --sage: #9bae97; /* soft sage — muted supporting */
  --mist: #cbd6c7; /* pale celadon for subtle fills */
  --moxa: #c8823c; /* moxa/turmeric gold — warm accent (herbal) */
  --clay: #b0563e; /* cinnamon clay — secondary accent, USE SPARINGLY */
  --line: rgba(30, 42, 36, 0.14); /* hairline rules on paper */
}
```

**Usage rules**

- Light sections: `--paper` bg, `--ink` text, `--jade` for links/eyebrows, `--moxa` for the single accent per section.
- Dark sections (Hero backdrop optional, "How we heal", CTA, footer): `--ink` bg, `--paper` text, qi-line and highlights glow in `--jade` → `--moxa`.
- `--clay` appears at most 2–3 times site-wide (e.g., an underline, a hover state). Restraint is the point.
- Add a subtle **paper grain**: a tiled SVG/`feTurbulence` noise overlay at ~3–5% opacity, `mix-blend-mode: multiply` on light sections. Keep it barely-there.

### Typography

Load from Google Fonts:

- **Display — Fraunces** (opsz variable; weights 300–900; include italic). Use with restraint for headings only. Turn on soft/high-contrast optical sizing for large sizes.
- **Body/UI — Instrument Sans** (400/500/600).
- **Mono/labels — Space Mono** (400/700) for point-codes, data, captions, and the "clinical chart" details.

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link
  href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300..900;1,9..144,300..900&family=Instrument+Sans:wght@400;500;600&family=Space+Mono:wght@400;700&display=swap"
  rel="stylesheet"
/>
```

**Type scale (fluid, use `clamp`)**

- Display XL (hero): `clamp(2.75rem, 7vw, 7rem)` Fraunces, weight 300–400, tight leading (~0.95), slight negative tracking. Mix in **italic Fraunces** for one emphasized word per headline (e.g., _listens_).
- H2 section: `clamp(2rem, 4.5vw, 4rem)` Fraunces 400.
- H3: `clamp(1.25rem, 2vw, 1.75rem)` Fraunces 500 or Instrument Sans 600.
- Body: `1.0625–1.1875rem` Instrument Sans 400, leading 1.6, `max-width: 62ch`.
- Eyebrow / point-code / caption: Space Mono, `0.75rem`, uppercase, letter-spacing `0.18em`, color `--jade` or `--moxa`.

**Voice for copy** (write from the visitor's side; plain, calm, specific; no filler): headline candidates —

- `Medicine that listens to the whole of you.` (recommended; italicize _listens_)
- or `Healing, one point at a time.`
- Sub: "Acupuncture, herbal medicine, and holistic care in Pasadena — supporting hormones, digestion, pain, fertility, and calm since 2013."

### Layout & spacing

- Max content width `1200px`; generous side gutters (`clamp(1.25rem, 5vw, 6rem)`).
- Section vertical rhythm: `clamp(5rem, 12vh, 10rem)` top/bottom padding.
- Baseline grid feel; align eyebrows and headings to a left rail where the qi-line lives.
- Border radius: soft but not bubbly — `2px–6px` on cards; some elements fully squared to keep an editorial edge.

### Motion principles

- Easing: default `power3.out` for entrances, `expo.out` for hero reveals. Durations 0.6–1.2s.
- **Orchestrate, don't scatter.** One strong hero load sequence + scroll reveals + the qi-line. Avoid animating everything.
- **`prefers-reduced-motion: reduce`**: disable Lenis, the qi-line scrub, split-text stagger, marquees, magnetic cursor, and parallax; content appears statically. Guard everything in a `gsap.matchMedia()` / `matchMedia` check.

---

## 5. File / section structure

Order of sections (each is an anchor target; each gets a point-code eyebrow from §3):

1. **Nav** (fixed)
2. **Hero** — thesis
3. **Trust strip** — stats + condition marquee
4. **What we heal** — the condition atlas
5. **How we heal** — philosophy (dark section)
6. **Services** — pinned/scroll cards
7. **Practitioners** — Zen & Kiera
8. **Testimonials** — review marquee/slider
9. **Book** — full-width CTA (dark)
10. **Contact + Footer**

Plus fixed layers: the **qi-line SVG**, **custom cursor** (desktop), **grain overlay**, **scroll-progress** hookup.

---

## 6. Section-by-section spec

### 6.1 Nav (fixed, `DU-20` not shown here)

- Left: wordmark "Healing with Zen" (Fraunces, small) — or a simple SVG mark (a needle + circle).
- Center/right: anchor links — `Heal`, `How`, `Services`, `Team`, `Reviews`, `Contact`.
- Right: **Book a Session** button → `https://healingwithzen.janeapp.com/` (moxa-gold fill, magnetic hover on desktop).
- Behavior: transparent over hero; on scroll past hero, condense (reduce height, add `--paper` bg + hairline bottom border, subtle blur). Animate with a ScrollTrigger toggle.
- Mobile: hamburger → full-screen overlay menu with staggered link reveal (GSAP). Include the phone number and Book button in the overlay.

### 6.2 Hero — eyebrow `DU-20 · PASADENA ACUPUNCTURE`

- Full viewport height. Left-aligned editorial composition.
- **Headline** (Display XL, split into lines): "Medicine that _listens_ to the whole of you." — _listens_ in italic Fraunces + moxa underline that draws in.
- Sub (Instrument Sans) + two CTAs: primary **Book a Session** (janeapp), secondary **Explore what we heal** (scrolls to §6.4).
- Trust line: `✧ Serving Pasadena since 2013 · Hundreds of 5-star experiences ✧` (Space Mono, small).
- **Visual:** an abstract SVG "acupuncture figure" or flowing meridian graphic on the right (`<!-- PHOTO SLOT: practitioner portrait -->`). Subtle parallax on scroll. Ambient floating point-nodes.
- **Load animation (timeline):** grain fades in → headline lines rise & unmask (stagger 0.08) → sub + CTAs fade up → qi-line draws its first segment → floating nodes fade in. Total ≈ 1.6s. `expo.out`.

### 6.3 Trust strip — eyebrow `ST-36 · WHY PATIENTS STAY`

- Three animated **count-up stats** (trigger on enter):
  - `2013` — "Caring for Pasadena since"
  - `500+` — "Five-star experiences" (use "Hundreds" if you prefer to avoid a hard number)
  - `30+` — "Conditions treated"
- Below: an infinite **marquee** of conditions (seamless GSAP loop, pause on hover): _Migraines · Anxiety · Fertility · Back pain · Insomnia · Digestion · Allergies · Vertigo · Neck & shoulder pain · Skin issues · Postpartum care · Cancer care support …_

### 6.4 What we heal — eyebrow `LI-4 · WHAT WE HEAL`

The old site's biggest strength buried in a menu. Present it as a clean, filterable **"atlas."**

- Five category tabs/pills (filter the grid; animate items in/out): **General Health · Pain · Women's Health · Mental Health · Other.**
- Grid items link to booking (or `#` placeholder). Full content:
  - **General Health:** Ear, Nose & Throat Issues; Digestion & Weight Loss; Allergies & Immune System Disorders; Skin Issues; Migraines & Headaches; Pediatrics.
  - **Pain:** Foot Pain; Neck & Shoulder Pain; Knee Pain; Back Pain.
  - **Women's Health:** Fertility; Pregnancy; Postpartum Care; Gynecological & Urogenital Issues; PMS.
  - **Mental Health:** Stress & Anxiety; Addiction / Substance Abuse Recovery; Insomnia.
  - **Other:** Cancer Care; Vertigo & Dizziness.
- Each item: point-code micro-label + name + a one-line plain-English benefit you write (e.g., "Fewer, milder migraine days — without another prescription.").
- Animation: items reveal on scroll (stagger); on filter change, FLIP-style fade/slide re-layout.

### 6.5 How we heal — eyebrow `REN-6 · OUR APPROACH` (DARK section, `--ink` bg)

- Big statement (paraphrase their real philosophy, don't copy verbatim): individualized health plans; a longer initial intake to understand health history and body constitution before gentle acupuncture; treating root causes, not just symptoms; physical, emotional, and spiritual care.
- Optional 3-step "process" (this content genuinely is a sequence, so numbering is allowed here): **01 Listen** (extended intake) → **02 Balance** (gentle, individualized acupuncture & herbs) → **03 Sustain** (a plan you can keep).
- The qi-line runs bright through this dark section — its visual peak.

### 6.6 Services — eyebrow `SP-6 · WHAT WE OFFER`

- Cards (or a pinned horizontal-scroll on desktop; stacked on mobile). Each: name, one-line description, `<!-- PHOTO SLOT -->`, hover lift + moxa accent.
- Full service list: **Acupuncture · Herbal Therapy · Cupping · Cosmetic Acupuncture · Facial Gua Sha · Microneedling · Online Telemedicine · Whole-Food Nutrition · Red Light Therapy · Earseeds · Gift Certificates.**
- Descriptions (write concise, benefit-first copy; examples):
  - Acupuncture — "Gentle, precise needling to move stuck qi and calm the nervous system."
  - Herbal Therapy — "Custom formulas that extend your results between visits."
  - Cupping — "Releases tension and improves circulation in tight, achy muscles."
  - Cosmetic Acupuncture / Gua Sha — "A natural glow and lift — no injectables."
  - Telemedicine — "Herbal and lifestyle guidance from wherever you are."

### 6.7 Practitioners — eyebrow `HT-7 · YOUR CARE TEAM`

- Two profiles, `<!-- PHOTO SLOT -->` headshots:
  - **Zen Tuan** — Acupuncturist, Herbalist & Clinical Director. Serving Pasadena since 2013.
  - **Kiera Layne** — Acupuncturist & Herbalist.
- Short bio line each (write warm, credible copy). Link "Read bio" (`#` placeholder).
- Animation: portraits reveal with a soft clip-path wipe; names split-reveal.

### 6.8 Testimonials — eyebrow `KI-3 · IN THEIR WORDS`

- These are the **client's own reviews** — include a curated set. **Trim/lightly edit each to ~1–2 sentences** and attribute by first name + last initial (as on the source). Use a horizontal auto-advancing slider or a two-row seamless marquee (pause on hover).
- Curated quotes (trimmed):
  - "The acupuncture and cupping instantly relieved so much of my back pain — and Zen really listens." — **Kevin X.**
  - "After one 15-minute treatment my neck's range of motion was dramatically better." — **Elsa C.**
  - "Two years in, she's relieved my osteoarthritis pain and improved my mobility." — **Susan D.**
  - "Chronic tinnitus that specialists couldn't touch — reduced dramatically after a few visits." — **Rob L.**
  - "Relaxing facial treatment, natural glow, and the best sleep I'd had in ages." — **Eli P.**
  - "A few sessions fixed a lower-back problem nothing else could." — **Fanny C.**
  - "Heated beds, calm music — more spa than clinic, and she truly listens." — **Kurt F.**
- Include an aggregate trust cue: "Hundreds of 5-star experiences across Google & Yelp." Link to Yelp/Google (real URLs in §7).

### 6.9 Book — eyebrow `LR-3 · START HEALING` (DARK, full-bleed)

- Large Fraunces line: "Ready to feel like yourself again?"
- One line of reassurance + **Book a Session** (janeapp) as the dominant button. Secondary: **Call (626) 377-9596** (`tel:` link).
- The qi-line resolves into a final glowing node here.

### 6.10 Contact + Footer — eyebrow `GB-20 · VISIT US`

- **Address:** 221 E. Walnut St, Ste 134, Pasadena, CA 91101 (link to Google Maps, §7).
- **Phone:** (626) 377-9596 → `tel:6263779596`
- **Email:** info@healingwithzen.com → `mailto:`
- Service-area line: "Serving Pasadena, San Marino, Arcadia, La Cañada, Altadena & the San Gabriel Valley since 2013."
- Newsletter signup (name + email; can be a non-functional demo form with a friendly success state, or point to their real provider later).
- Socials (real): Instagram, Facebook, YouTube, Yelp, Google (URLs in §7).
- Legal: Terms & Conditions, Privacy Policy links; `© 2026 Healing with Zen — Acupuncture & Herbal Remedies`.
- Optional: NCCAOM / award badges (`<!-- BADGE SLOT -->`).

---

## 7. Real data / links (use these exactly)

- **Book:** `https://healingwithzen.janeapp.com/`
- **Phone:** `(626) 377-9596` → `tel:6263779596`
- **Email:** `info@healingwithzen.com`
- **Address:** `221 E. Walnut St, Ste 134, Pasadena, CA 91101`
- **Google Maps:** `https://www.google.com/maps/place/Healing+with+Zen`
- **Instagram:** `https://www.instagram.com/healingwithzen/`
- **Facebook:** `https://www.facebook.com/healingwithzen`
- **YouTube:** `https://www.youtube.com/channel/UC7W3C3Rfu8Wk9tsqA-pGwFw`
- **Yelp:** `https://www.yelp.com/biz/healing-with-zen-pasadena`

---

## 8. GSAP animation checklist

- [ ] Lenis smooth scroll wired to `ScrollTrigger.update` + `gsap.ticker` (desktop; disabled on reduced-motion).
- [ ] Hero load timeline (unmask headline lines, fade CTAs, draw underline).
- [ ] **Qi-line**: full-page SVG path, `stroke-dashoffset` scrubbed by total scroll progress; point-nodes pulse on section enter; gradient jade→moxa.
- [ ] Custom line/word splitter → stagger-reveal every section heading on enter.
- [ ] Count-up stats on enter.
- [ ] Two seamless marquees (conditions, testimonials), pause on hover.
- [ ] Nav condense on scroll; mobile overlay stagger.
- [ ] Magnetic buttons + custom cursor (desktop pointer only; `matchMedia('(pointer:fine)')`).
- [ ] Parallax on hero visual + subtle section-image parallax.
- [ ] "What we heal" filter re-layout animation.
- [ ] Services pinned horizontal scroll on desktop (optional), stacked on mobile.
- [ ] Everything wrapped in `gsap.matchMedia()`; full reduced-motion fallback.

---

## 9. Accessibility & quality floor

- Semantic landmarks (`header/nav/main/section/footer`), one `<h1>` (hero), logical heading order.
- All interactive elements keyboard-reachable with **visible focus rings** (jade outline, 2px).
- Color contrast ≥ 4.5:1 for body text (verify moxa/clay on paper — darken if needed for text; they're fine as accents/large text).
- `prefers-reduced-motion: reduce` fully honored (see §4).
- Alt text on all meaningful SVG/images; decorative SVG `aria-hidden`.
- Forms: labelled inputs, error + success states in the interface's own voice.
- Respsonsive breakpoints: `≤640` (mobile), `641–1024` (tablet), `≥1025` (desktop). Test the qi-line and marquees at each.
- No layout shift from font loading (`font-display: swap`, size-adjust if needed).

---

## 10. GitHub Pages deployment

1. Repo root contains `index.html` (+ `/assets` if split). Add an empty `.nojekyll` file so paths with underscores aren't processed.
2. Push to `main`.
3. Repo **Settings → Pages → Build and deployment → Source: Deploy from a branch → `main` / `/ (root)`**.
4. Site publishes at `https://<user>.github.io/<repo>/`. Because all asset paths are **relative** and CDN links are absolute `https://`, it works under a subpath with no config.
5. Optional custom domain later via `CNAME` file.

Include a short `README.md` with these steps and a one-line project description.

---

## 11. Build order (suggested for Claude Code)

1. Scaffold `index.html`, tokens, fonts, grain, Lenis + GSAP wiring, reduced-motion guard.
2. Nav + Hero (+ load timeline).
3. Qi-line system (get the scrub + nodes solid early — it threads all sections).
4. Trust strip, What we heal, How we heal.
5. Services, Practitioners, Testimonials.
6. Book CTA, Contact/Footer, forms.
7. Polish pass: magnetic cursor, marquees, parallax, mobile menu.
8. Self-critique pass: screenshot, check spacing/contrast/focus, remove one accessory. Then ship.

---

## 12. Acceptance criteria

- Loads with no console errors; passes reduced-motion (no animation, full content).
- Qi-line visibly draws through the whole page and lights section nodes.
- Every CTA points to the real janeapp booking URL; phone/email/address correct.
- Distinct from the generic cream-serif-terracotta template — jade/ink/moxa identity is unmistakable.
- Fully responsive; keyboard-navigable; deploys clean to GitHub Pages.
