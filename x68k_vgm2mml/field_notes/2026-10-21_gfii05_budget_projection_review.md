# GFII regression and budget-oriented projection review (2026-10-21)

## Scope

User requested investigation/planning only before selecting the next implementation.
No production code, thresholds or tests changed. No VGM conversion, compilation,
playback or full private regression was run. Analysis consumed saved PCM IR and
metadata only. New ignored analysis: outputs/diagnostics/gfii05_budget_plan_20261021/.

## User regression

The similar-bank run exported 9/10. GFII05 first timed out at the default limit,
then failed with timeout1200. GFII04/06 still report insufficient runtime PCM
buffer; their standard mixed output retained22 chunks each and merged none.
GFII04 low reduced the payload to680822bytes/11chunks, still insufficient in
user playback. This is not a96-slot problem. Other reported bank outcomes:

| Song | Representatives | Retained | Retained payload bytes | Retained playbacks | Omitted |
|---|---:|---:|---:|---:|---:|
| GFII03 |60|60|123779|669|0|
| GFII05 |161|96|162522*|1369*|146*|
| GFII07 |89|89|206089|1201|0|
| GFII08 |116|96|240834|1587|33|
| GFII09 |69|69|120794|175|0|
| GFII10 |50|50|121597|296|0|

*GFII05 values are saved target selection, not a completed PDX or playback.

## Actual GFII05 failure

Saved _errors/GFII05.vgm.log ends with Refusing to replace modified or unowned
PCM block output: GFII05.sample_182.15625_1Hz.5456f18804e0b592.wav.
The file exists but pcm_blocks/GFII05.manifest.json does not. The diagnostic
writer writes candidate WAVs before its final ownership manifest, so timeout
can leave exactly this orphan state. This is consistent with the earlier
interruption; it is not evidence that the user edited the WAV. Core reduction
and96-slot selection had already completed. A fresh outdir avoids this specific
ownership conflict, but final conversion/playback success remains unverified.

The report's Detail captures usage text rather than the actual final error;
future failure reporting should preserve the meaningful error message.

## Source identity versus generated assets

Saved data:571 encoded assets,577 source views:348 ROM views and229 trajectory
variants. ROM views contain65 distinct byte hashes/integer address ranges,
331 exact fractional ranges,13 delta values and11 end-page identities.
The11 boundaries are not proof of11 instruments. Integer-range equality is
not waveform equality: fractional cursor, source delta, stop and held controls
matter. Current161 representatives:102 ROM-view assets and59 trajectory variants.
Omitted65:42 ROM-view assets and23 trajectory variants. Thus the user's premise
that generated assets outnumber original materials is supported, but65 is not
an established lossless bank size.

## Read-only candidate screen

For each omitted representative, inspected same-rate retained representatives
with normalized spectral/envelope descriptors and onset-gain feasibility.
All65 have gain-feasible candidates; a candidate with tonal estimate within
100cents or unknown pitch exists for all65. Selected30 candidates have unknown
pitch. Spectral distance median0.260/p900.693/max0.877; normalized-envelope
median0.277/p900.547/max0.842. These are planning distances, not current pass
metrics or perceptual equivalence. No all-original-member/event/gate validation
was performed for these proposed replacements. Simply loosening all thresholds
would allow materially different percussion.

## Proposed order, not implemented

1. Repair interrupted optional preview generation. Reuse an orphan only when
   its bytes exactly match newly expected output; preserve modified files.
   Make optional preview I/O errors warnings after core MML/PCM/PDX generation,
   while retaining fatal source/hash/IR consistency validation. Retain pass CSVs.
2. Offer an explicit target-budget approximation mode, provisionally budget96.
   Keep defaultsimilar quality bounds. Choose representatives globally using
   usage/error and storage, then substitute near sounds for otherwise omitted
   events. Every original event must be evaluated directly against the final
   representative with actual quantized gain/gate; no chained approximation.
   Report deliberate over-bound replacements separately and compare omissions.
   At most96slots does not guarantee PDX total bytes, eight-voice allocation,
   compiler size or runtime buffer fit. A byte budget is a separate future knob.
3. Share reusable bank infrastructure with MSM6258/Delta-T. Native MSM6258
   already separates samples/playbacks and deduplicates exact encoded data;
   do not infer original instruments from a software-mixed ADPCM stream.
   Preserve predictor/step/nibble/reset conditions. Delta-T needs source-specific
   finite independently restartable views before existing mixed-waveform
   packaging, preserving memory generation, rate, gain and decoder epochs.
   PSG PCM waits for sounding-unit/drum cutout analysis; generic target helpers
   may be reused afterward. This concerns one96-slot bank, not true multi-bank
   PDX support, whose format/runtime scope requires separate verification.

Architect read-only review agreed with this order and these constraints.
User chooses next implementation after this report. No Git stage/commit/push;
external notes are separate and unrelated existing changes were preserved.
