# Healing with Zen — Website

A single-page marketing site for **Healing with Zen**, an acupuncture & Traditional Chinese Medicine clinic in Pasadena, CA. Static, no build step, deployed to GitHub Pages.

The design concept is **"The Meridian"**: a continuous qi-line draws itself down the page as you scroll, lighting acupuncture-point nodes at each section. Palette is jade / pine-ink / moxa-gold on warm rice-paper — grounded in the actual world of Chinese medicine, not the generic cream-and-terracotta wellness template.

## Run locally

No build, no dependencies. Just open the file:

```bash
open index.html          # macOS
# or serve it:
python3 -m http.server   # then visit http://localhost:8000
```

Libraries (GSAP + ScrollTrigger, Lenis) and Google Fonts load from CDN, so the animated experience needs a network connection on first load. Content is fully readable offline / with JS disabled.

## Tech

- **Single self-contained `index.html`** — inline `<style>` and `<script>`, vanilla JS only.
- **GSAP 3 + ScrollTrigger** — hero timeline, qi-line scrub, scroll reveals, count-ups, marquees, magnetic buttons, pinned services scroll.
- **Lenis** — smooth scroll (wired into `ScrollTrigger.update` + `gsap.ticker`).
- **No images** — all visuals are inline SVG/CSS. `<!-- PHOTO SLOT -->` comments mark where real photography drops in.
- **Accessible** — semantic landmarks, single `<h1>`, visible jade focus rings, and a full `prefers-reduced-motion` fallback (static content, no animation).

## Deploy to GitHub Pages

1. This repo root contains `index.html` and an empty `.nojekyll` (so underscore paths aren't processed).
2. Push to `main`.
3. **Settings → Pages → Build and deployment → Source: Deploy from a branch → `main` / `/ (root)`**.
4. Publishes at `https://<user>.github.io/<repo>/`. All asset paths are relative and CDN links are absolute `https://`, so it works under a subpath with no config.
5. Optional custom domain later via a `CNAME` file.

## Real details

- **Book:** https://healingwithzen.janeapp.com/
- **Phone:** (626) 377-9596
- **Address:** 221 E. Walnut St, Ste 134, Pasadena, CA 91101
