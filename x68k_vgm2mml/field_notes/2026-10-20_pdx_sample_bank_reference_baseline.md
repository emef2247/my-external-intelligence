# Reusable PDX bank investigation — 2026-10-20

## User direction

mucom88 work is set aside. Investigate traditional sample-bank MML/PCM IR in
x68k, using private Galaxy_Force MDX/PDX as the desired output structure.
Clarification: same reuse/bank semantics, NOT identical reference PCM bytes or
slot layout. User pasted the complete shared ChatGPT conversation after the
share URL failed. Its external technical assertions remain reference claims,
not verified specifications. Do not switch to Z-MUSIC/MPCM or redesign the
whole IR first. Inspect existing data, compare current output, identify gaps,
then make the smallest grounded projection change.

Shared URL could not be read through web fetch or the in-app browser, which
redirected to an account/browser-check error. Pasted content is now available.
No production source/test changes, conversion or playback performed. Existing
static audit utilities read the explicitly supplied reference files only.

## Reference and source

Reference root:
I:/wsl/repositories/emef2247/test/x68k_vgm2mml/tests/fixtures/local_only/opm_msm6258/mdx_pdx/Sega/Galaxy_Force

Ten MDX files (including GF1S/GF1S_ variant) all refer to GFS.PDX. The one
96-entry table has 16 populated slots,1..16, all distinct encoded hashes.
PDX total69675 bytes =768 table +68907 payload. Sample lengths713..17616 bytes,
median2765. All ranges valid; payload ends exactly at file EOF.
All MDXs statically decode completely with 16-track layout and valid bindings.
Parsed PCM rate operands are all4. This does not independently establish
physical PCM behavior or the actual resident driver version.

GF7S:1411 expanded requested note events over P/Q/R/S; slot7 (750 bytes) is
referenced1108 times. GF3S:3141 note events across P..U, the maximum6 active PCM
tracks in this collection. Do not call these raw attacks or infer acoustic
peak polyphony: audit counts include finite-repeat expansion and a bounded
song-loop traversal; holds/gates/exhaustion require separate interpretation.
The screenshot is user-provided MMDSP playback evidence; no new listening here.

Artifacts, private/ignored:
outputs/diagnostics/pdx_sample_bank_20261020/reference_summary.json
outputs/diagnostics/pdx_sample_bank_20261020/<MDX stem>/{audit.json,pdx_slots.csv,pcm_bindings.csv,controls.csv,endings.csv}

Matching SegaPCM source VGMs GFII01..10 exist under local_only/opm_segapcm/
vgmrips.net/GFII. No x68k outputs/opm_segapcm/vgmrips.net/GFII folder exists.
No new private conversion was run to invent a current-output comparison.
Musical/sample equivalence between that source set and the hand-authored
reference arrangement has not been established.

## PCM8 article

assets/ohx/Oh!X_1992-06.pdf: article printed pages52–56 (PDF pages54–58).
All five relevant pages rendered and visually inspected. The historical driver
is a per-voice ADPCM->PCM decode, volume/rate conversion, mix, PCM->ADPCM output
pipeline, with preserved state between interrupt blocks. Five discrete source
rates and sixteen volume levels are described. It does not define PDX packing
or prove today's MDX compiler exposes every direct-call feature. DMA double
buffers and lookup tables are not PDX library-size limits.
PDF54/56 first and55/57/58 subsequent renderings, plus text, remain ignored in
the diagnostic folder. PDF text has OCR errors; figures/page images corroborate
used claims. No article text copied into public documentation.

## Current-code distinction

Native MSM6258 already has PcmSample/PcmPlayback and actual byte+codec dedup,
with independent-start/reset eligibility. Thus do not say the whole project
has no sample bank. The derived Sega route currently mixes native channels
then uses long mono chunks, with gain/rate/phase baked in and final-clock
length resampling. The typed target plays only P with Q-W inactive.

The needed change is source/view identification and reusable target assets
before mixing, then event scheduling/allocation. Gate duration must not
reshape stored assets. Pan/volume controls should remain events where supported.
Rate-specific derived assets may be necessary with discrete MDX rates; reuse
those variants, not arbitrary whole-song chunks. Runtime pitch/loop/offset
capabilities need their own target proof.

## Review and next milestone

Architect reviewed the supplied measurements/current-code evidence; its local
additional read was denied, so no claim of independent full code audit.
No general recovery of original samples/voices from PCM8-mixed B7 output.
ADPCM reuse must consider predictor/step/nibble/reset state. Prefer a small
SegaPCM bank-route milestone using retained ROM/control provenance, existing
IR where meaningful, multi-track typed target/assembler checks, and public
repeat/duration/pan/volume/overlap cases. Keep fallback and capacity diagnostics.

Public proposal: docs/pcm_sample_bank_design.md (explicitly design, not support).
Next: compare existing compatible generated artifacts where available, establish
finite stable-source-view contract and MDX control subset before implementing.
Do not resume suspended enhancer or mucom work; do not copy private samples into
git. No Git stage/commit/push.
