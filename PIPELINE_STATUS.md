# Pipeline Status

Operational handoff only. `LEADS.md` and `OUTREACH_LOG.md` remain the source of truth.

- Current phase: FIX phase complete (2026-07-13) — image-crop defect fixed and re-verified. **PASS.** Proceeding to DEPLOY / DRAFT.
- Last trusted commit: b551f09 (first commit, "Build Carl Church demo site: Home, Work, Contact", made at end of build session). Fix commit for the crop defect made this session (see git log for exact hash).
- Known untrusted state: none remaining. The builder's original QA_REPORT.md "PASS" on image/content match missed that BUILD_BRIEF's claimed clean 50/50 crop of 3 composite slider images was wrong (verified native pixel *dimensions* only, not actual seam position); independent review caught it. This session re-cropped `sculpture-bust-dome.jpg` (from `Slider-3-1440x720.jpg`, 0–523), `sfx-lucy-bust.jpg` (from `Slider-4-1440x720.jpg`, 0–601), and `sfx-skulls.jpg` (from `Slider-2-1440x720.jpg`, 0–651) at the reviewer-measured true seams, using the `/tmp/carlchurch_orig/slider{2,3,4}.jpg` cache from the review session — verified byte-identical (md5) to a fresh `curl` of the live source URLs before trusting it. All 3 corrected images visually re-inspected: clean, no bleed. `work.html`'s `<img>` width/height attributes updated (523×720, 601×720, 651×720). Full detail in `QA_REPORT.md` → Fix Verification.
- Next exact action: none outstanding on QA — site is deploy-ready. Proceed with GitHub repo creation + Pages, then outreach draft.
- Deploy URL: see DEPLOY section below once live (this session).
- Outreach state: see OUTREACH section below.
- Flags for Alex: (1) Carried over from build: LEADS.md says "Pickering, North Yorkshire" but the business's own site only states "North Yorkshire" (no town) — the built site deliberately omits "Pickering" since it wasn't independently reconfirmed (see BUILD_BRIEF Do Not Claim). If you have separate confirmation Pickering is current/correct, it's an easy one-line addition to contact.html — non-blocking.
