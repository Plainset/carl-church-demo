# Build Brief

Keep this compact. Add only sourced facts and assets actually used or deliberately rejected.

## Contact
- Email: carlchurchsculptor@gmail.com
- Email source URL: https://carlchurch.co.uk/ (footer contact block, `mailto:carlchurchsculptor@gmail.com`, appears 2x on homepage) — matches LEADS.md entry exactly.
- Rechecked date: 2026-07-13 (curl fetch of live homepage HTML, HTTP 200)
- Phone: +44 (0)7840 451590 — sourced from same on-site contact block (`carlchurch.co.uk`), matches LEADS.md.
- Address: No street address published on carlchurch.co.uk. Site copy only states "based in North Yorkshire, England, UK." LEADS.md lists "Pickering, North Yorkshire" — that town-level detail is **not** independently confirmed from the business's own site/socials in this session, so the built site uses only "North Yorkshire" and does not assert "Pickering." See Do Not Claim.

## Business State Check
- Status: still open / actively trading (online-only studio/workshop presence; no physical shopfront claimed).
- Checked sources (all via curl, 2026-07-13):
  - https://carlchurch.co.uk/ — HTTP 200, loads, `dateModified` in page schema is 2024-07-19 (site itself last edited then); several subpages show newer `dateModified` (see below), so the business is maintaining the site periodically.
  - https://carlchurch.co.uk/sculpting-sculpture-and-resin-castings/ — HTTP 200, `dateModified` 2025-08-12 (edited within the last year).
  - https://www.instagram.com/carlchurchbird/ — HTTP 200 (reachable).
  - https://www.instagram.com/carlchurchsculptor/ — HTTP 200 (reachable).
  - https://www.facebook.com/carlchurchscultpor — HTTP 200 (reachable).
  - https://amazon.co.uk/dp/B097SN9GKD (his book) — HTTP 301 (redirects, live Amazon listing).
  - Companies House search for "Carl Church" — 0 results. He is not a registered company; consistent with a self-employed sole-trader artist (site bio: "Self employed artist for over twenty years"). No Companies House evidence exists to check, so this is not a red flag — noted per instructions rather than fabricating a filing.
  - No closure, relocation, or online-only pivot notice found anywhere. Best-available signal is a combination of: (a) 2024–2025 on-site edit timestamps, (b) reachable, distinctly-named social profiles matching the site's own linked handles, (c) museum-held work, published book, and 26-year career already documented in LEADS.md and reconfirmed directly on-site in this session.
- Build decision: proceed.

## Pre-build defect re-verification (LEADS.md claims), curl'd 2026-07-13
All three previously logged defects are still live on https://carlchurch.co.uk/ (confirmed via `curl -sL`):
1. **Unreplaced Qode Interactive template footer** — site footer widget area literally reads "198 West 21th Street, Suite 721 / New York NY 10010 / Email: youremail@yourdomain.com / Phone: +88 (0) 101 0000 000 / Fax: +88 (0) 202 0000 001", plus a "More Links" column of dead demo links to `http://demo.qodeinteractive.com/bridge3/`, and a "© Copyright Qode Interactive" line — none of it relates to Carl Church.
2. **Duplicate/redundant nav & contact markup** — the real phone/email block (`+44 (0)7840 451590` / `carlchurchsculptor@gmail.com`) is duplicated verbatim in two separate DOM locations on the homepage (both a `.footer` widget column and a second nested `.footer` widget column further down), alongside a full second copy of the social/book-purchase link list — confirms the "duplicate" complaint is real markup duplication, not a one-off rendering glitch.
3. **Embedded Flickr feed showing unrelated protest photos** — the homepage still embeds a `wpb_flickr_widget` pulling from `flickr.com/photos/71865026@N00` (a third party, "markjsebastian"), displaying images explicitly titled "#7410 No Justice No Peace" etc. — confirmed unrelated to Carl Church's work.

This satisfies the BUILD checklist's pre-build re-verification step; the pitch ("your own site still runs on unedited template junk that undersells 26 years of award-winning, museum-held work") stands.

## Page Plan
- Scope: 3-page default — Home, Work (Portfolio), Contact.
- Pages: `index.html`, `work.html`, `contact.html`.
- Reason for any extra page: none added — 3 pages fully cover the verified content (bio/highlights, full portfolio across taxidermy/sculpture/film-SFX/book, contact).

## Pitch Hook
- Verified observation: carlchurch.co.uk still shows unedited "Qode Interactive New York" WordPress theme-demo footer content (fake US address, `youremail@yourdomain.com`, `+88` fax numbers), duplicated contact/nav markup, and an embedded Flickr widget pulling a stranger's "No Justice No Peace" protest photos instead of Carl's own work — actively undermining a portfolio that includes museum-held taxidermy, a published manual, and film-industry sculpture work.
- Source URL: https://carlchurch.co.uk/ (curl'd 2026-07-13).

## Allowed Facts
| Fact | Source URL | Used where |
|---|---|---|
| International award-winning bird taxidermist and established sculptor, based in North Yorkshire, England, UK | https://carlchurch.co.uk/ | Home hero/intro |
| Self-employed artist for over twenty years (site copy); LEADS.md rounds this to "26 years' experience" | https://carlchurch.co.uk/ | Home, Work intro |
| Qualified as a bird specialist in 2002 with the UK Guild of Taxidermists | https://carlchurch.co.uk/ | Home about, Work |
| Competed at World Taxidermy Show, America, 2003 — won Best Professional Bird | https://carlchurch.co.uk/ | Home highlights, Work |
| Competed again 2005 — won Best Professional Bird & Best Competitors award | https://carlchurch.co.uk/ | Home highlights, Work |
| Taxidermy supplied to the National History Museum of California; specimens in major European collections | https://carlchurch.co.uk/ | Home highlights, Work |
| 2008 — started special-effects sculpture work for the film industry and top UK advertising agencies | https://carlchurch.co.uk/ | Work (film/SFX section) |
| 2008 — exhibited at the Baltic Centre, Gateshead, in a "couples exhibition" paired with an embalmer | https://carlchurch.co.uk/ | Work intro (used briefly, career timeline) |
| Made 5 dodo recreations for the Great Dodo Exhibition at Kendal Museum, Cumbria, with 3 leading dodo experts giving the recreations national recognition | https://carlchurch.co.uk/ | Work (taxidermy section, dodo image) |
| 2012 — appeared on Channel 4's "Four Rooms" with a cased Andean condor | https://carlchurch.co.uk/ | Home highlights, Work |
| Published "Bird Taxidermy: The Basic Manual" (2013), sold worldwide, 6000+ copies, 420+ colour photos, one of few modern bird taxidermy manuals available | https://carlchurch.co.uk/ and https://carlchurch.co.uk/bird-taxidermy-the-basic-manual/ | Home highlights, Work (book section) |
| Since 2023, working with Tennants Auctioneers of Leyburn | https://carlchurch.co.uk/ | Work intro |
| Produces hand-carved wooden sculptures (life-size to small mixed-medium), casts in resin and pewter, subjects spanning human, equestrian, medieval and gothic forms | https://carlchurch.co.uk/ | Work (sculpture section) |
| Book purchase link (Amazon UK) | https://amazon.co.uk/dp/B097SN9GKD | Work (book section), Contact |
| Facebook: "Carl Church Sculptor" | https://www.facebook.com/carlchurchscultpor | Contact footer |
| Facebook: "Carl Church Bird Taxidermy" | https://www.facebook.com/BirdTaxidermy | Contact footer |
| Instagram: @carlchurchbird | https://www.instagram.com/carlchurchbird/ | Contact footer |
| Instagram: @carlchurchsculptor | https://www.instagram.com/carlchurchsculptor/ | Contact footer |
| YouTube channel | https://www.youtube.com/channel/UCbyF7G1HzguOv52ZhX7GzXQ | Contact footer |
| Email carlchurchsculptor@gmail.com | https://carlchurch.co.uk/ (mailto links) | Contact, header/footer everywhere |
| Phone +44 (0)7840 451590 | https://carlchurch.co.uk/ | Contact, footer |

## Do Not Claim
| Claim or uncertainty | Reason |
|---|---|
| "Pickering, North Yorkshire" as a specific town | Only in LEADS.md, not confirmed on carlchurch.co.uk or his socials during this session's curl checks. Site itself only says "North Yorkshire, England, UK." Site copy is used verbatim instead. |
| carl@birdtaxidermy.co.uk | Appears once on-site as a `mailto:` behind an "on Instagram" label — clearly a mismatched/stale template artifact (icon says Instagram, link is a different, unverified email). Not used; only the confirmed carlchurchsculptor@gmail.com is used. |
| Any specific film/advert titles for the SFX work | Site only says "special effects for the film industry and some of the UK's top advertising agencies" with no named productions. Copy stays generic — no invented film credits. |
| Twitter/X, Google+, Tumblr social icons from the Qode footer | These point to generic `twitter.com`, `plus.google.com`, `tumblr.com` — unreplaced template placeholders, not real Carl Church profiles. Not used. |
| Specific gallery/exhibition dates beyond what's stated | Only the Baltic Centre (2008), Kendal Museum Dodo Exhibition, and Channel 4 Four Rooms (2012) are named on-site; no other exhibitions invented. |

## Asset Manifest
All images sourced directly from `carlchurch.co.uk`'s own WordPress media library (`/wp-content/uploads/...`), fetched via curl 2026-07-13. Visually inspected individually — no watermarks on any image used (Carl's own studio-shot portfolio photography). None reused from third-party press/editorial coverage or the broken Flickr feed.

| File | Source URL | Native size | License/credit | Watermark checked | Intended section | Copy match |
|---|---|---|---|---|---|---|
| hero-condor.jpg | https://carlchurch.co.uk/wp-content/uploads/2021/08/Andean-Condor-taxidermy-680x900-1.jpg | 680×900 | Own site, artist's own work photography | yes/none found | Home hero inset | Andean condor taxidermy — generic caption, no fabricated provenance |
| portrait-carl.jpg | https://carlchurch.co.uk/wp-content/uploads/2021/11/carl-homepage.jpg | 600×783 | Own site | yes/none found | Home — about the artist | Portrait of Carl with a hand-carved wood horse-head sculpture |
| dodo-recreation.jpg | https://carlchurch.co.uk/wp-content/uploads/2021/08/dodo-940x900-2.jpg | 940×900 | Own site | yes/none found | Work — taxidermy section | One of the dodo recreations for the Kendal Museum exhibition (verified fact) |
| carving-gargoyle.jpg | https://carlchurch.co.uk/wp-content/uploads/2021/08/01_gargoyle-900x1400-1.jpg | 900×1400 | Own site | yes/none found | Home teaser + Work — wood carving | Hand-carved wood gargoyle |
| carving-horse-head.jpg | https://carlchurch.co.uk/wp-content/uploads/2021/08/01_equestrian-art-1400x900-2.jpg | 1400×900 | Own site | yes/none found | Work — wood carving | Equestrian wood carving |
| pewter-boar.jpg | https://carlchurch.co.uk/wp-content/uploads/2021/08/Boar-800x600-1.jpg | 800×600 | Own site | yes/none found | Home teaser + Work — pewter/bronze | Cast pewter boar relief |
| taxidermy-puffins.jpg | https://carlchurch.co.uk/wp-content/uploads/2021/08/0021_bird-taxidermy-680x900-1.jpg | 680×900 | Own site | yes/none found | Work — taxidermy section | Puffin pair taxidermy |
| taxidermy-white-eye.jpg | https://carlchurch.co.uk/wp-content/uploads/2021/08/01_carl-church-taxidermist-680x900-1.jpg | 680×900 | Own site | yes/none found | Work — taxidermy section | Small songbird (white-eye) taxidermy |
| taxidermy-harpy-eagle.jpg | https://carlchurch.co.uk/wp-content/uploads/2021/08/harpy-eagle-600x1200-1.jpg | 600×1200 | Own site | yes/none found | Work — taxidermy section | Harpy eagle taxidermy |
| taxidermy-golden-eagle.jpg | https://carlchurch.co.uk/wp-content/uploads/2024/07/Slider-3-1440x720.jpg (right half, cropped locally with Pillow — no content alteration beyond a straight vertical split of the existing 2-photo slider composite) | 720×720 (cropped from 1440×720) | Own site | yes/none found | Home teaser + Work — taxidermy section | Golden eagle taxidermy |
| taxidermy-duck-rock.jpg | https://carlchurch.co.uk/wp-content/uploads/2024/07/Slider-4-1440x720.jpg (right half, cropped locally, same method) | 720×720 (cropped from 1440×720) | Own site | yes/none found | Work — taxidermy section | Bird taxidermy piece on rock/snow base |
| sculpture-bust-dome.jpg | https://carlchurch.co.uk/wp-content/uploads/2024/07/Slider-3-1440x720.jpg (left half, cropped locally, same method) | 720×720 (cropped from 1440×720) | Own site | yes/none found | Work — sculpture/resin section | Sculpted bust under glass dome |
| sfx-chimp.jpg | https://carlchurch.co.uk/wp-content/uploads/2024/07/chimpazee-1-scaled.jpeg | 2015×2560 | Own site | yes/none found | Work — film/SFX section | Prosthetic/SFX creature-head sculpture |
| sfx-clown.jpg | https://carlchurch.co.uk/wp-content/uploads/2024/07/Slider-2-1440x720.jpg (right half, cropped locally, same method) | 720×720 (cropped from 1440×720) | Own site | yes/none found | Work — film/SFX section | Prosthetic clown make-up/SFX piece |
| sfx-skulls.jpg | https://carlchurch.co.uk/wp-content/uploads/2024/07/Slider-2-1440x720.jpg (left half, cropped locally, same method) | 720×720 (cropped from 1440×720) | Own site | yes/none found | Work — film/SFX section | Sculpted skull props |
| sfx-lucy-bust.jpg | https://carlchurch.co.uk/wp-content/uploads/2024/07/Slider-4-1440x720.jpg (left half, cropped locally, same method) | 720×720 (cropped from 1440×720) | Own site | yes/none found | Work — film/SFX section | Prosthetic primate-head bust ("Lucy") |
| manual-spread-1.jpg | https://carlchurch.co.uk/wp-content/uploads/2021/08/Manual-Spread1-1200x800-1.jpg | 1200×800 | Own site | yes/none found | Work — book section | Interior spread from "Bird Taxidermy: The Basic Manual" |
| manual-spread-2.jpg | https://carlchurch.co.uk/wp-content/uploads/2021/08/Manual-Spread2-1200x800-1.jpg | 1200×800 | Own site | yes/none found | Work — book section | Interior spread from "Bird Taxidermy: The Basic Manual" |

Note on the 720×720 crops: the homepage/gallery hero sliders combine two separate photos side-by-side into one 1440×720 file. Cropping at the vertical midpoint (720px) with Pillow separates the two original photos cleanly with no distortion, upscaling, or content alteration — verified visually after crop. All display sizes in the built CSS are capped at-or-below each image's native resolution (see Design Notes).

## Design Notes
- Palette: warm near-black charcoal background (`oklch` low-lightness neutral), ivory/parchment text, a bronze/gold accent (echoes cast pewter/bronze work + old museum labels), deep forest-green secondary (natural-history-museum association). Distinct from a generic "dark mode" — warm, not blue-black.
- Typography: "Fraunces" (serif, characterful, hand-carved feel) for display headings; "Inter" for body/UI text. Loaded via Google Fonts `<link>` (no build step).
- Image layout pattern: mixed — hero/portrait/teaser images use `aspect-ratio` + `object-fit: cover` wrapper pattern; all display widths capped at or under native pixel width (checked per breakpoint in QA) to avoid upscaling.
- Risk notes: several images are 680–900px native width; capped hero/teaser display sizes accordingly (max ~440–620px rendered width at desktop) rather than forcing full-bleed banners, per AGENTS.md's full-bleed quality-bar rule. `sfx-chimp.jpg` is high-res (2015×2560) so it can run larger without any upscale risk.

## Builder QA
- Contrast: PASS after 1 fix — see QA_REPORT.md. `.pipeline/qa/contrast-audit.js` run via Playwright/Chromium at mobile/tablet/desktop on all 3 pages found the header "Enquire" button text was rendered near-invisible (measured 1.05–1.27:1 contrast) due to a CSS specificity bug (`.main-nav a` beating `.btn-primary`'s color). Fixed by rescoping to `.main-nav ul a` in `assets/css/style.css`. Rechecked contrast post-fix: 7.46–9.90:1.
- Upscale mobile/tablet/desktop: PASS — see QA_REPORT.md. 0 violations, 0 broken images, 0 aspect mismatches across all 9 page×breakpoint runs.
- Broken images: PASS — see QA_REPORT.md.
- Manual checks: PASS — see QA_REPORT.md (text-overflow with real content, mobile nav, image/content match, fabricated-claims spot check all clean).
