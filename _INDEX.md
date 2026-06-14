# freegangsheetmaker

Rebuilt, optimized single-file version of **freegangsheetmaker.com** (DTF.STREAM / ScreenPrintGPT). Client-side only, no backend.

| File | Description |
|------|-------------|
| `index.html` | Complete free DTF gang sheet maker. MaxRects bin-packing (Best Short Side Fit) with rotation, per-design quantity, transparent-border trim, custom roll width / gap / DPI, multi-page splitting to ZIP, 300 DPI print-ready PNG export. Modern responsive UI plus SEO/AI-SEO meta, WebApplication + FAQPage schema, and on-page FAQ/how-to content. JSZip loaded from CDN. |
| `PUBLISH.md` | Plain-English publishing guide. GitHub + DigitalOcean App Platform auto-deploy, DNS pointing for freegangsheetmaker.com, the ongoing Claude Code update workflow, troubleshooting, and a Hostinger manual-upload fallback. |
| `.gitignore` | Standard ignores. Folder is a git repo, committed and push-ready. |

Created **2026-06-13**. Packing logic verified (0 overlaps, 0 out-of-bounds) across mixed-75, exact-width, and single-design test cases.
