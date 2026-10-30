# GFII finite-bank, continuation and PSG feedback plan — 2026-10-21

## Scope and evidence

User requests improvement proposals for GFII04/05/06/07/08, with collection
priority GFII > TBLADE > TBOUR, and feedback to --psg-output pcm. User closes the
TBL01 subjective pitch inquiry. This turn reads existing diagnostic metadata,
implementation, prior field notes and upstream driver source. No new local_only
conversion, test, compilation or listening; no production source/test changes.
Public design: docs/pcm_bank_continuations.md (proposal only).

Existing runtime traces show zero sample_loop events and zero loop-enabled gate
starts for all five songs. These are one traversed source-song iteration; VGM
header song loops are separate and current regression used --no-song-loop.
Thus actual internal sample-loop support is not a prerequisite for these five.

| Song | Current PDX bytes | Populated slots | Enable gates | Initially nonaudible then potentially audible | Peak enabled overlap | Audible address/rate/gain writes | Candidate range/rate descriptions |
| --- | ---: | ---: | ---: | ---: | ---: | --- | ---: |
| GFII04 | 1,362,412 | 22 | 1667 | 162 | 8 | 670/558/1547 | 46 |
| GFII05 | 615,668 | 11 | 852 | 52 | 6 | 249/275/206 | 20 |
| GFII06 | 1,281,216 | 22 | 1529 | 215 | 8 | 177/214/762 | 21 |
| GFII07 | 673,104 | 11 | 886 | 105 | 5 | 30/40/262 | 9 |
| GFII08 | 414,718 | 7 | 398 | 39 | 4 | 104/120/558 | 24 |

These gate counts are hardware enable-to-stop intervals, not musical attacks.
Potentially audible means nonzero delta and at least one nonzero gain, not a
waveform/RMS measurement; delta zero can also hold DC. Enabled overlap is an
upper bound, not acoustic polyphony. Description counts use programmed snapshots,
coarse end pages, bank masks and changing live addresses; they are neither
verified instruments nor estimates of final stored assets. Do not predict a
final PDX size from them. Abstract inventory is saved at
outputs/diagnostics/gfii_bank_plan_20261021/source_inventory.json.

## Interpretation and per-song priorities

All five currently use whole-song mix fallback. Muted initialization advances
source cursors; zero delta or loop flags at opening must not reject all later
sound. Resolve effective native-frame writes in order, preserving fractional
cursor, ROM generation, bank and actual path. Address low/high/control writes
are not independent attacks. Derive source-linked continuation, reposition and
unresolved boundaries without changing native State or Segments.

GFII07 is the smallest interpretation proving case (9 candidate descriptions,
30 address and 40 rate changes). Prove finite view/suffix reuse and bounded
held-control variants here, then GFII05 (20 candidates, 6 enabled voices).
GFII04/06 need the same model across more events and up to 8 enabled voices;
there may be no spare track for a mixed fallback. GFII08 has only 4 enabled
voices but 558 audible gain writes and 9 ROM generations: continuity, envelope
variants and generation identities matter more than the number of channels.
All five remain ahead of TBLADE and TBOUR work.

Cache independently encoded finite full/suffix views with exact effective
address/fraction, rate, ROM identity and target profile. Do not cut ADPCM bytes
and assume decoder state. Keep asset length separate from scheduled gate length;
final MDX clock alignment must not stretch an asset independently for each use.
Consider per-asset target rates within the quality profile with actual resampling
and explicit pitch/duration/bandwidth policy; just changing F is not conversion.

Ordinary MXDRV held @v/p/F changes do not imply live PCM control. Prefer common
asset scale plus verified onset gain; bound and reuse baked control/rate variants.
Do not create a new full-duration waveform for every slice. Any localized mix
must have disjoint source/time membership from banked material, sharing P-W's
8-track budget. No whole-song fallback layered over the same banked sources.

## Target-volume defect found by static inspection

Current py/segapcm_bank.py maps source level linearly into direct raw 128..255.
In MXDRV2.06+17 Rel.X5-S, SetPCMVolume clears bit7, interprets the remainder as
attenuation, and at index >=43 clears pan. Example source roughly half-level
maps raw192, index64, therefore muted rather than half-level. Fix before bank
expansion, using an explicit compatible driver/gain profile and inverse tables.

PCMVolumeTable selects 16 levels; X68Sound PCM8VOLTBL is
2,3,4,5,6,8,10,12,16,20,24,32,40,48,64,80. Lowest gain is not zero: source silence
requires separate mute/omission and restoration at the next attack. Example
half of maximum80 is40, level12, MXDRV attenuation9..10 (raw137..138).
Retain desired/effective gain and error in pass CSVs. MADRV acoustic agreement
is still unmeasured. Current whole-song mix uses raw128, so this bank-only bug
is not the explanation for the five existing mix buffer warnings.

Static references: mxdrv17.s L000c66, SetPCMVolume, PCMVolumeTable and
volume/pan/frequency commands; X68Sound pcm8.h/global.h. Downloaded references
are under outputs/diagnostics/gfii_bank_plan_20261021/references/.
MXDRV source SHA256: aca2de497ac4ed1f9d06036b3eeb1b6fe15f6f1e502805766b82092a220eb51b.
Primary URL: https://github.com/vampirefrog/x68kd11s/blob/master/sound/mxdrv/2.06%2B17_Rel.X5-S/mxdrv17.s

## Actual sample loops, after finite GFII cases

Separate initial prefix, multi-cycle reusable loop body and phase-specific tail.
PDX offset/length alone has no native loop points. Repeated notes reset the
ADPCM decoder, unlike a source continuous loop; ties do not restart exhausted
assets. Evaluate seam/reset errors, accumulated multi-repeat duration/phase drift
and final stop. Short source loops must not become one MDX attack per period.
Muted or stopped-before-loop paths remain finite when proven from traversal.
Keep unsupported portions explicit rather than claiming exact internal looping.

## PSG feedback and source-wide target reuse

Share immutable assets, sample/view/playback identities, global slot binding,
P-W allocation, force-capacity diagnostics and typed MML/PDX generation. Preserve
native MSM6258 extraction; already-mixed B7 audio cannot generally be demixed.
This is a small shared boundary, not a universal IR rewrite.

PSG has no ROM instrument: channel-resolved continuous audio must preserve
oscillator phase, shared noise/EG and filter history. Current probe stores mono
PSG_calc output. The pinned emu2149 quality1 averages several internal updates;
reading its final ch_out[3] does not reconstruct the averaged per-channel audio.
Possible public proof: synchronized masked cores receiving every write in the
same order and preserving shared state, then check extraction/rounding/filter
behavior against the original continuous baseline. Do not silently change quality.

Start banking repeatable finite tone/phrase material whose initial state and
evolution are equivalent. Evolving noise/EG may need localized rendering.
Software-envelope evidence remains musical-unit based; do not reset per note or
assume live held @v reproduces envelopes. Common target machinery does not
promise every PSG source can be compressed into a small reusable bank.

## Proposed execution and validation boundary

1. Correct onset volume and explicit silence, with authored target expectations.
2. Implement muted-init/path-aware finite views and exact suffix starts; prove
   GFII07/05-shaped authored cases, then broader GFII04/06/08 trajectories.
3. Add bounded held-control variants and disjoint fallback/allocation diagnostics.
4. Address genuinely traversed internal sample loops, then PSG continuous-channel
   extraction and reuse using the same target infrastructure.

Use only public/authored automated inputs; inspect source/part/asset/event/binding
passes and final MML/PDX separately. User performs private regression/listening.
No fixed byte threshold guarantees native buffer fit. No implementation of these
extensions was made this turn; existing finite-bank checkpoint remains intact.
No stage, commit or push in either repository. External notes contain unrelated
prior changes and must be handled separately if commits are later requested.
