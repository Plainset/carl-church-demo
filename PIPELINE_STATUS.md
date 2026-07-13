# Pipeline Status

Operational handoff only. `LEADS.md` and `OUTREACH_LOG.md` remain the source of truth.

- Current phase: DEPLOYED (2026-07-13). Fix, re-QA, and deploy all complete this session.
- Last trusted commit: 0251739 ("Fix image-crop bleed: re-crop 3 composite-derived images at true seam"), pushed to `Plainset/carl-church-demo` main branch.
- Known untrusted state: none remaining. The builder's original QA_REPORT.md "PASS" on image/content match missed that BUILD_BRIEF's claimed clean 50/50 crop of 3 composite slider images was wrong (verified native pixel *dimensions* only, not actual seam position); independent review caught it. This session re-cropped `sculpture-bust-dome.jpg` (from `Slider-3-1440x720.jpg`, 0–523), `sfx-lucy-bust.jpg` (from `Slider-4-1440x720.jpg`, 0–601), and `sfx-skulls.jpg` (from `Slider-2-1440x720.jpg`, 0–651) at the reviewer-measured true seams, using the `/tmp/carlchurch_orig/slider{2,3,4}.jpg` cache from the review session — verified byte-identical (md5) to a fresh `curl` of the live source URLs before trusting it. All 3 corrected images visually re-inspected: clean, no bleed. `work.html`'s `<img>` width/height attributes updated (523×720, 601×720, 651×720). Full detail in `QA_REPORT.md` → Fix Verification.
- Next exact action: none outstanding — pipeline complete for this business pending Alex's review of the outreach draft (or the flags below).
- Deploy URL: https://plainset.github.io/carl-church-demo/ — confirmed live (HTTP 200), title and content verified via curl, all 3 fixed images confirmed loading (HTTP 200) at their corrected crop dimensions.
- Outreach state: draft only, not sent — see `OUTREACH_LOG.md` (top-level session updates this) for exact status.
- Flags for Alex: (1) Carried over from build: LEADS.md says "Pickering, North Yorkshire" but the business's own site only states "North Yorkshire" (no town) — the built site deliberately omits "Pickering" since it wasn't independently reconfirmed (see BUILD_BRIEF Do Not Claim). If you have separate confirmation Pickering is current/correct, it's an easy one-line addition to contact.html — non-blocking.
