# Free Gang Sheet Maker — project guide

Context for Claude (and Cowork) working on this project. Read this first.

## What this is
A free, browser-based **DTF gang sheet maker / layout optimizer**. Users upload transparent PNGs; the tool bin-packs them into a print-ready 300 DPI gang sheet using the least film, then exports a PNG (or a ZIP for multi-page). Everything runs client-side — **artwork is never uploaded**. Goal: rank for "gang sheet maker / builder", get cited by AI engines, and (later) sell ad space, so usage analytics matter.

- **Live site:** https://freegangsheetmaker.com/
- **Repo:** https://github.com/marcotteink/freegangsheetmaker (public, branch `main`)
- **Owner:** Matt Marcotte (matt@marcotte.ink), founder of DTF.STREAM & ScreenPrintGPT.

## Architecture
Static site, no build step. Plain HTML/CSS/JS.

- `index.html` — the **tool homepage**. Fully self-contained: inline `<style>` and one inline `<script>` (IIFE) hold the entire app (upload, MaxRects bin-packer, render to canvas, 300 DPI PNG export via a hand-written `pHYs` chunk, lightbox, sample-designs generator). Also contains all homepage SEO content + JSON-LD.
- `guide.css` — shared stylesheet for the guide/About pages (NOT used by index.html, which is self-contained).
- `gang-sheet-builder/index.html`, `how-to-build-a-gang-sheet/index.html`, `dtf-gang-sheet-sizes/index.html` — SEO guide pages (topical cluster). Clean URLs via folder/index.html.
- `about/index.html` — author/About page (ProfilePage + Person schema for Matt Marcotte).
- `robots.txt` (welcomes AI crawlers), `sitemap.xml`, `llms.txt`, `favicon.svg`, `og-image.png` (+ `og-image.svg` source), `CNAME` (= freegangsheetmaker.com), `<indexnow-key>.txt`.

## Hosting & deploy
- **GitHub Pages** serves `main`, auto-deploys on every push. Custom domain via the `CNAME` file; HTTPS enforced.
- **DNS at GoDaddy:** four apex A records → GitHub IPs `185.199.108–111.153`; `www` CNAME → `marcotteink.github.io`.
- **Deploy = just push:** `git add -A && git commit && git push`. Pages rebuilds in ~1 min. Confirm a build with `gh api repos/marcotteink/freegangsheetmaker/pages/builds/latest -q .status` (→ `built`).

## Analytics
- **GA4** Measurement ID `G-ZG8477726M` (same property used since the prior Replit deployment, so data is continuous). Loaded in every page's `<head>`.
- Homepage fires custom events via `track(name, params)`: `upload_designs`, `load_samples`, `resize_design`, `toggle_trim`, `expand_thumbnail`, `render_layout` (rich utilization payload), `download`. **Never send artwork or file names** — counts and sheet params only.

## SEO / AI-SEO (GEO)
- Verified in **Google Search Console** (URL-prefix; `google-site-verification` meta in every page's head).
- **IndexNow** set up: key file `<key>.txt` at root (key in `.indexnow-key`). After publishing new/changed URLs, submit with:
  `POST https://api.indexnow.org/indexnow` body `{host, key, keyLocation, urlList}`.
- Structured data: Organization, WebSite, WebApplication, HowTo, FAQPage on home; BreadcrumbList + Article/HowTo + FAQ on guides; ProfilePage + Person on /about/.
- **Personal-brand entity:** Matt Marcotte marked up as `Person` (#matt, url https://marcotte.ink) — creator/author of the tool and `founder` of DTF.STREAM + ScreenPrintGPT. Visible attribution in the homepage "About the maker" block, footer, guide bylines, and /about/.

## Conventions & gotchas
- **Don't fabricate** reviews/`aggregateRating` or any credential/brand claim — Google penalty risk, and dishonest.
- **Keep on-page FAQ count == FAQPage schema count** (Google requires FAQ schema to match visible content).
- **og-image regen** (no ImageMagick/Homebrew on the owner's Mac): edit `og-image.svg` (1200×1200, content in the centered 630 band) → `qlmanage -t -s 1200 -o . og-image.svg` → `sips -c 630 1200 og-image.svg.png` → rename to `og-image.png`.
- Validate JSON-LD after edits: `JSON.parse` each `<script type="application/ld+json">` block.
- **Local-machine only (not Cowork):** `gh` lives at `~/.local/bin/gh` (no Homebrew); preview via `python3 -m http.server` from the repo dir. In Cowork these don't apply — use the environment's own git/preview tooling.

## Open items / TODO
- **`www` HTTPS cert:** DNS fixed (`www → marcotteink.github.io`); GitHub re-provisioning the SAN cert to cover `www`. Verify `https://www.freegangsheetmaker.com` resolves cleanly.
- **Add social `sameAs`:** Matt's LinkedIn / X / Instagram URLs into every `Person` `sameAs` array to strengthen the entity (currently marcotte.ink + the two brands only).
- **Backlinks (biggest ranking lever):** link to freegangsheetmaker.com from DTF.STREAM and ScreenPrintGPT, plus Reddit/forum/directory mentions.
- Ranking for head terms is an authority+time game — expect weeks; no #1 guarantee.

## Working in Cowork
This repo is connected to GitHub, so Cowork can work on it directly: open the project, select the `marcotteink/freegangsheetmaker` repo, and Cowork reads this file automatically. Describe the change in plain English; Cowork edits, commits, and pushes; GitHub Pages redeploys in ~1 minute.
