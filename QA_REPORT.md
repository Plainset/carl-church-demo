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

## Independent Reviewer Verification (2026-07-13)

Per REVIEW.md, treated the above builder-reported PASS as a map, not proof, and re-verified independently rather than cross-checking filenames against the manifest alone (see sibling Wheelwrights incident: a manifest-only "PASS" missed two swapped images). Actually opened every one of the 18 images used on the site and visually compared each to its caption/alt text and adjacent copy. Also independently re-ran the shared audits against a fresh local server (port 4262, separate from the builder's port 4261) via the `.pipeline/.qa-scratch` Playwright install, and independently recomputed gradient-text contrast in-browser via canvas `fillStyle` (not just trusting the builder's numbers).

| Check | Result | Evidence |
|---|---|---|
| Contrast (independent recompute) | PASS | Re-ran `contrast-audit.js` fresh (not copy-pasted from builder) across 3 pages × 3 breakpoints: 0 violations everywhere. Independently recomputed the 5 `needsManualCheck` gradient-text pairs via in-page canvas `fillStyle`→pixel sampling: `.btn.btn-primary` text vs gradient 7.46:1 / 9.90:1, `.cta-band h2` vs gradient 6.85:1 / 10.67:1, `.cta-band p` vs gradient 6.84:1 — all comfortably above WCAG AA. Confirms the builder's header-button fix holds. |
| Upscale/broken/aspect (independent, with forced lazy-load) | PASS | Wrote a custom Playwright scroll-to-bottom-then-audit script (run-audit.js alone doesn't trigger `loading="lazy"`; confirmed 4 images reported `notYetLoaded` on a naive run before adding the scroll step). After proper lazy-load triggering: 0 violations, 0 broken, 0 aspect mismatches across all 3 pages × 3 breakpoints (index: 6 images, work: 16, contact: 0). |
| Mobile layout | PASS | 375px width, all 3 pages: nav toggle click sets `.main-nav.is-open` + `aria-expanded="true"` correctly; `document.documentElement.scrollWidth === clientWidth` (no horizontal overflow) on every page. |
| Scroll-reveal | PASS (after investigation) | A full-page screenshot of work.html initially showed large blank-looking gaps between sections — investigated rather than assumed a bug. Root cause: `page.screenshot({fullPage:true})` doesn't give `IntersectionObserver` real scroll-dwell time for every element, and a synthetic fast step-scroll test also under-triggered it (16/21 `data-reveal` elements visible). A direct `scrollTo(bottom)` + 1.5s settle test (matching how a real user's scroll actually settles) showed 21/21 `data-reveal` elements correctly reach `.is-visible`. Not a real defect — confirmed via a second, more decisive test rather than left ambiguous. |
| Fabricated facts | PASS (1 verified, not fabricated) | Spot-checked the odd-sounding "National History Museum of California" (not a real institution's exact name) directly against a fresh `curl` of `https://carlchurch.co.uk/`: the phrase appears verbatim in the source's own bio copy. Correctly carried over from the source, not invented or "corrected" into something equally wrong — no action needed. |
| Image/content match — **actually opened every image, not just filenames vs. manifest** | **1 real defect found** | See Blocking Issues below. All 15 non-composite images (hero-condor, portrait-carl, dodo-recreation, carving-gargoyle, carving-horse-head, pewter-boar, taxidermy-puffins, taxidermy-white-eye, taxidermy-harpy-eagle, sfx-chimp, manual-spread-1/2) were visually opened and match their captions/alt text/adjacent copy cleanly. The 6 locally-cropped 720×720 images (from `Slider-2/3/4-1440x720.jpg`) needed closer inspection because BUILD_BRIEF claimed a clean midpoint (x=720) split "verified visually after crop" — that claim was checked against the real pixels, not taken on trust, and found to be wrong for half of them. |

## Blocking Issues (found in independent review)

| Issue | Evidence | Required fix |
|---|---|---|
| **3 of the 6 locally-cropped 720×720 images contain a visible strip of a *different, unrelated* photo bled in from the adjacent panel of the same slider composite — the "clean midpoint split" BUILD_BRIEF claims was not actually true.** Fetched the 3 live source composites (`Slider-2/3/4-1440x720.jpg`) directly from `carlchurch.co.uk` and measured the real seam between the two side-by-side photos in each (column-wise brightness-jump analysis, confirmed visually): actual seams are at x≈523 (Slider-3), x≈601 (Slider-4), x≈651 (Slider-2) — **not** x=720 as BUILD_BRIEF assumed. Cropping each "left half" at 0–720 therefore captures extra pixels past the real seam: <br>• `sculpture-bust-dome.jpg` (used on Work page, Sculpture section, caption "Sculpted bust, under glass"): right ~197px (**27% of the 720px frame**) is actually the golden eagle taxidermy photo — antlers, grass, and grey backdrop from an unrelated bird-taxidermy piece, visible inside a photo captioned as sculpture. This is a category mismatch (taxidermy content appearing in the sculpture section), not just a cosmetic glitch. <br>• `sfx-lucy-bust.jpg` (Work page, Film/SFX section, caption "Prosthetic bust study — film SFX"): right ~119px (**16.5%**) is a strip of `taxidermy-duck-rock.jpg`'s grey backdrop/base. <br>• `sfx-skulls.jpg` (Work page, Film/SFX section, caption "Sculpted skull props — film SFX"): right ~69px (**9.6%**) is a strip of the clown prosthetic's tan/blue polka-dot collar. <br>The other 3 crops (`taxidermy-golden-eagle.jpg`, `taxidermy-duck-rock.jpg`, `sfx-clown.jpg` — all "right half" crops starting at x=720, which in every case is already past the real seam) are clean; confirmed no bleed on those three. | Re-crop the 3 affected files from the already-downloaded source composites at the *actual* measured seam instead of the assumed midpoint: `sculpture-bust-dome.jpg` → crop `Slider-3-1440x720.jpg` at 0–523 (not 0–720); `sfx-lucy-bust.jpg` → crop `Slider-4-1440x720.jpg` at 0–601; `sfx-skulls.jpg` → crop `Slider-2-1440x720.jpg` at 0–651. Update the `<img>` width/height attributes to match the new (narrower) crop dimensions and re-run `upscale-audit.js` at mobile/tablet/desktop afterward — the resulting crops are still well within native resolution at the site's actual rendered display sizes, so no upscale risk is expected, but it must be re-verified rather than assumed. Alternatively, if a clean re-crop still looks too tight/awkwardly framed after cropping at the true seam, cut the affected image and run that portfolio slot as text-only per AGENTS.md's fallback rule, rather than ship a spliced image. |

## Fix Verification (2026-07-13, FIX phase)

Re-cropped all 3 affected files from the originals cached at `/tmp/carlchurch_orig/slider{2,3,4}.jpg` this session — verified byte-identical (md5) to a fresh `curl` of the live `carlchurch.co.uk/wp-content/uploads/2024/07/Slider-{2,3,4}-1440x720.jpg` URLs before using them, so no risk of using stale/wrong-session cache content.

| File | Source composite | Old crop (wrong) | New crop (correct seam) | Result |
|---|---|---|---|---|
| `sculpture-bust-dome.jpg` | `Slider-3-1440x720.jpg`, left half | 0–720 (bled golden-eagle content from the right-half photo) | 0–523 → 523×720 | Re-opened and visually inspected: clean, bust-under-dome only, no eagle/antler/grass bleed. |
| `sfx-lucy-bust.jpg` | `Slider-4-1440x720.jpg`, left half | 0–720 (bled `taxidermy-duck-rock.jpg` backdrop) | 0–601 → 601×720 | Re-opened and visually inspected: clean, "Lucy" bust under dome only, no duck-rock bleed. |
| `sfx-skulls.jpg` | `Slider-2-1440x720.jpg`, left half | 0–720 (bled clown prosthetic's polka-dot collar) | 0–651 → 651×720 | Re-opened and visually inspected: clean, both skull props only, no clown-collar bleed. |

`work.html`'s `<img>` `width`/`height` attributes updated to match the new native crop dimensions (523×720, 601×720, 651×720 respectively — `height` unchanged at 720, `width` narrowed). Display CSS is unaffected: these sit in `.piece.ratio-square .frame { aspect-ratio: 1/1 }` with `img { object-fit: cover }`; since each new crop's width is now the limiting axis (narrower than tall), `cover` scales width-to-fit and crops only top/bottom — it does not reintroduce any horizontal (side) cropping, so the fixed seam holds at every breakpoint.

Re-ran `.pipeline/qa/upscale-audit.js` at mobile (375px)/tablet (768px)/desktop (1440px) against a fresh local server (port 4263, via the `.pipeline/.qa-scratch` Playwright install — chrome-devtools MCP and WebFetch were permission-denied this session) with a scroll-to-bottom-then-settle step first to force all `loading="lazy"` images to load before auditing (per the same method the independent reviewer used):

| Page | Breakpoint | Checked | Violations | Broken | Aspect mismatches | Not yet loaded |
|---|---|---|---|---|---|---|
| work.html | 375 / 768 / 1440 | 16/16 each | 0 | 0 | 0 | 0 |
| index.html | 375 / 768 / 1440 | 6/6 each | 0 | 0 | 0 | 0 |

No upscale risk from the narrower crops (523/601/651px native width, well above the ~350px max rendered width these grid cells occupy even at desktop). The other 3 composite crops (`taxidermy-golden-eagle.jpg`, `taxidermy-duck-rock.jpg`, `sfx-clown.jpg`) were confirmed clean by the independent reviewer and were not touched.

## Verdict
**PASS.** The image-crop bleed defect is fixed and independently re-verified (visual re-inspection of all 3 corrected images plus a full 6-run upscale/broken/aspect audit across both affected pages at all 3 breakpoints). Contrast, mobile layout, scroll-reveal, and fabricated-claims checks were already independently verified clean in the prior review pass and are unaffected by this fix. Cleared to deploy.
