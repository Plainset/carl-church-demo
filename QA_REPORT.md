# QA Report

Use exact pass/fail evidence. "Looks fine" is not a result.

## Pages Checked
- index.html
- work.html
- contact.html

All checks run against a live local server (`python3 -m http.server`) via Playwright/Chromium, at three breakpoints: mobile 375×812, tablet 768×1024, desktop 1440×900. `.pipeline/qa/contrast-audit.js` and `.pipeline/qa/upscale-audit.js` were injected via `page.evaluate()` on each page/breakpoint combination (9 runs each). Images were forced to full-load (scroll-to-bottom + `waitForFunction` on `img.complete && naturalWidth>0`, then scroll back to top) before each upscale-audit run, since several images use `loading="lazy"`.

## Audit Results
| Check | Result | Evidence |
|---|---|---|
| Contrast audit | PASS (after 1 fix) | 0 hard violations across all 9 page×breakpoint runs, both before and after the fix below. `needsManualCheck` count per run: index 4-5 (gradient buttons + `.cta-band` gradient), work 4-5 (same shared header/CTA), contact 0-1 (shared header button only). All `needsManualCheck` items independently verified below. |
| Upscale mobile | PASS | index.html: 6/6 images checked, 0 violations, 0 broken, 0 aspect mismatches, 0 notYetLoaded. work.html: 16/16 images checked, 0/0/0/0. contact.html: 0 images on page (text-only), 0/0/0/0. |
| Upscale tablet | PASS | index.html: 6/6, 0/0/0/0. work.html: 16/16, 0/0/0/0. contact.html: 0 images, 0/0/0/0. |
| Upscale desktop | PASS | index.html: 6/6, 0/0/0/0. work.html: 16/16, 0/0/0/0. contact.html: 0 images, 0/0/0/0. |
| Broken images | PASS | `brokenImages: []` on every one of the 9 runs (naturalWidth never 0 on a loaded `<img>`). |
| Aspect mismatch advisory | PASS | `aspectMismatches: []` on every run — no `object-fit: fill` images at all; all images use the `aspect-ratio` wrapper + `object-fit: cover` pattern per AGENTS.md. |

## Manual Checks
| Check | Result | Notes |
|---|---|---|
| Text on photo | PASS | No text is overlaid directly on a photo anywhere in this build (hero uses a two-column layout, not text-on-image); not applicable. |
| Gradient/::before backgrounds | PASS (1 fix required) | Flagged elements: `a.btn.btn-primary` (3 instances: header "Enquire", hero "View the work", hero "Commission a piece"/CTA-band "Get in touch") over `linear-gradient(in oklch, var(--accent), var(--accent-strong))`, and `.cta-band h2`/`p` over `linear-gradient(in oklch, var(--forest), color-mix(...))`. Manually computed via canvas `fillStyle` → sRGB → WCAG contrast ratio (see below) — see **Blocking Issues** for the one real defect found and fixed; all other gradient text passed cleanly (6.8–10.7:1). |
| Image/content match | PASS | Cross-checked all 18 images against `BUILD_BRIEF.md`'s Asset Manifest: native pixel sizes on disk (`PIL Image.size`) match the manifest exactly for every file (e.g. `sfx-chimp.jpg` 2015×2560, `taxidermy-harpy-eagle.jpg` 600×1200, all four 720×720 crops). Captions/alt text match subject matter (condor, dodo, gargoyle, eagle, boar, etc.), no third-party or mismatched photos used. |
| Fabricated claims | PASS | Spot-checked on-page copy against `BUILD_BRIEF.md`'s Allowed Facts table — all claims used (26+ years, 2× World Taxidermy Show Best Professional Bird, National History Museum of California, Kendal Museum dodo recreations, Channel 4 Four Rooms 2012, 6,000+ copies of the manual, Tennants Auctioneers since 2023) trace to a sourced row. `Do Not Claim` items ("Pickering", the stale `carl@birdtaxidermy.co.uk` address, named film credits, Twitter/Google+/Tumblr icons) are absent from all three pages — confirmed by grep. |
| Mobile layout | PASS | 375px screenshot (`/tmp/carlchurch_mobile.png`, not committed) shows single-column hero, working hamburger nav, no overflow. Nav toggle programmatically tested: click sets `.main-nav` class to `main-nav is-open` and `aria-expanded="true"`. |
| Text-overflow (real content) | PASS | Rendered `.contact-list li`, `.contact-list .v`, `.footer-contact`, `.footer-contact a`, `.social-row a` at 375px with the actual longest real strings (`carlchurchsculptor@gmail.com`, `+44 (0)7840 451590`, `Facebook — Carl Church Sculptor`, etc.) — `scrollWidth <= clientWidth` on every box, 0 overflow violations. |

## Blocking Issues
| Issue | Evidence | Required fix |
|---|---|---|
| Header "Enquire" button (and any future `.btn-primary` placed inside `<nav class="main-nav">`) rendered near-invisible text: `.main-nav a { color: var(--ink-dim) }` (specificity 0,1,1) was overriding `.btn-primary { color: var(--accent-ink) }` (specificity 0,1,0) regardless of source order, because the button `<a>` is a direct child of `.main-nav`, not just of `.main-nav ul`. Computed contrast of the actual (buggy) rendered color `oklch(0.78 0.03 80)` against the gradient background `oklch(0.72 0.13 70)`→`oklch(0.8 0.14 75)` was **1.05–1.27:1** — fails WCAG AA's 4.5:1 normal-text threshold by a wide margin (button text is 14px/600-weight, not large text). Same specificity issue also broke the intended `:hover` color. | Fixed in this session — see Fixed Verification below. |

## Advisory Issues
- None outstanding.

## Fixed Verification
| Issue | Fix | Recheck result |
|---|---|---|
| `.main-nav a` selector (specificity 0,1,1) overriding `.btn-primary`'s intended text color for any button placed directly inside `<nav class="main-nav">` | `assets/css/style.css`: rescoped the nav-link rule and its hover/aria-current variant from `.main-nav a` / `.main-nav a:hover, .main-nav a[aria-current="page"]` to `.main-nav ul a` / `.main-nav ul a:hover, .main-nav ul a[aria-current="page"]` — matching the actual DOM intent (style the `<ul>` links only; the `.btn-primary` sibling keeps its own color rules uncontested) | Re-computed contrast after the fix via canvas `fillStyle`→sRGB: all three `.btn.btn-primary` instances (header "Enquire", hero "View the work", CTA "Get in touch") now render `oklch(0.18 0.02 60)` as intended, contrast **7.46:1** (vs. gradient start) and **9.90:1** (vs. gradient end) — both comfortably above WCAG AA 4.5:1. Full contrast+upscale audit re-run across all 9 page×breakpoint combinations post-fix: 0 violations, 0 broken images, 0 aspect mismatches, 0 not-yet-loaded, on every run. Visually confirmed via desktop (1440px) and mobile (375px) screenshots — button text now clearly legible. |

## Additional pre-QA verification (this session)
- Email re-verified live at the exact `LEADS.md`/`BUILD_BRIEF.md` source URL: `curl -sL https://carlchurch.co.uk/` → HTTP 200, `carlchurchsculptor@gmail.com` present ×2, phone `+44 (0)7840 451590` present ×2. Matches the built site exactly.
- All external links used on the Contact page independently re-checked live: both Facebook pages, both Instagram profiles, the YouTube channel, and the Amazon book listing all returned HTTP 200 via `curl -sL -o /dev/null -w "%{http_code}"`.
- All 18 images' on-disk native pixel dimensions (via Python PIL) verified to match `BUILD_BRIEF.md`'s Asset Manifest exactly — no undocumented cropping/resizing since the brief was written.
- Scroll-reveal `IntersectionObserver` in `assets/js/main.js` correctly implements the AGENTS.md standing rule: checks `getBoundingClientRect()` per `[data-reveal]` element before observing and adds `is-visible` immediately for anything already in the viewport at load.
- Standing CSS/HTML defaults spot-checked in `assets/css/style.css`: global `box-sizing: border-box`, `overflow-wrap: anywhere` + `word-break: break-word` + `min-width: 0` on `.contact-list .v`, `font-variant-numeric: tabular-nums` on `.stat .num` and `.process-list li::before`, `@view-transition { navigation: auto; }` present, `linear-gradient(in oklch, ...)` and `color-mix(in oklch, ...)` used for brand gradients/tints, `:hover` on stable wrapper (`.card`) animating a child (`.card .frame img`) rather than the hover-owning element itself, `grid-template-rows: subgrid` on `.card` for aligned card rows, `aspect-ratio` wrapper + `object-fit: cover` pattern used consistently for all images.

## Verdict
PASS
