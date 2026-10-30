# PSG/SCC curve projection: source/history audit (2026-10-24)

User requested reviewing MSX field_notes and the supplied Konami MSX-SCC driver
analysis before choosing implementation. This turn is investigation/design only:
no conversion source code or options changed, no local_only conversion/test run,
no staging/commit/push. A proposed English design was saved in
msx_vgm2mml/docs/psg_scc_curve_projection_plan.md.

## Materials and limits

Existing software_envelopes.md and psg_scc_performance.md explain the converter,
but no standalone text summary of the supplied book was found in searched docs.
The relevant supplied photographs were read directly, rather than assuming an
unlocated summary had been reviewed. Do not claim full-book coverage: the photo
collection jumps from p17 to p51. The focused source pages were:
- IMG_5001 / IMG_5003: p4-5, multiple driver generations.
- IMG_5004..IMG_5007: p6-9, comparative command inventories.
- IMG_5008: p10, note/register/macro modes and channel applicability.
- IMG_5009: p11, note/update operation and frame cadence.
- IMG_5010: p12, register/macro percussion and duration behavior.
- IMG_5011: p13, Gradius2 context (not production note/parameter evidence).
- IMG_5012: p14, volume controls, noise/mixer and hardware envelope.
- IMG_5013: p15, multi-phase software envelope and one-tick rising attack.
- IMG_5014: p16, pitch LFO, delay and period behavior.
- IMG_5015: p17, mode selection context.
- IMG_5016: p51, software envelope diagrams and duration-linked release.
- IMG_5017: p52, driver-dependent release-entry restrictions.
- IMG_5018: p53, amplitude LFO plus pitch LFO diagrams.
- IMG_5019: p54, pitch bend and waveform morphing.
- IMG_5020/IMG_5022..IMG_5026: pp55-60, MGSDRV appendix context only;
  authored tables/score data were not imported as converter rules or fixtures.

The scan directory is under ../../assets/
ENCYCLOPEDIA_of_Consumer_Game_Sound_Drivers_-_konami_MSX-SCC_Soun_-.
The book's analysis is evidence for its driver families, not a universal chip or
arbitrary-VGM specification. No source tables or private score text are copied.

## Confirmed current code and prior decisions

py/psg_scc_performance.py keeps immutable native source references, audibility
gates and ordered typed per-Segment snapshots, but _notes still splits at pitch
writes, rising volume, settings changes and resets. Filtering only within those
legacy ScoreNotes cannot recover complete attack or vibrato units. SoundGate is
inferred audibility, not observed Key-On; even positive sub-tick silence matters.
Settings include original tone periods; do not lose detune to improve matches.

LoopFirstEnvelopeBank prepares complete-unit exact structural keys before bank
selection. The bank shares exact curves/prefixes and has 32 entries including
constant zero ID. Envelope IDs do not define source-unit identity. Current
volume curves are 60-Hz holds and are not rescaled with musical normalization.

History reviewed: 2026-09-29_gra2_compression.md,
2026-09-29_macro_compression.md, 2026-09-29_psg_scc_periods.md,
2026-09-30_nemesis_macro_capacity.md,
2026-10-03_psg_scc_pre_envelope_structure.md,
2026-10-03_structured_macro_followup.md,
2026-10-23_psg_scc_performance_ir.md and frontend catch-up notes.
Important prior constraints: complete units before loop/envelope selection;
state-safe loops only for equal output commands; no reference-tempo/envelope
injection; shared bank across chips; macro text reductions did not change
compiled track bytes in controlled experiments. Loops/source-size gains were
mixed by input. Byte capacity and text size must remain separate metrics.

## Design decision

Refine the previous proposal: derive and classify complete sounding-unit
candidates before approximating internal curves. A rising attack, amplitude
LFO or period LFO can explain volume rises/pitch writes without new notes;
neither regular cadence nor a curve fit proves the original score boundary.
Keep ambiguous existing partitions as fallback. No zero-volume-is-keyoff rule.

First deliverable: full volume curves and explicit protected/continuation/
retrigger/unresolved boundary evidence in a separate target projection; exact
reuse on supported fixed-tone/fixed-wave cases. Next deliverable: opt-in bounded
volume approximation with independent psg-state-ms/scc-state-ms, default off.
Time threshold alone cannot remove intentional one-frame effects. Require
separate volume/integrated error limits, source-sample time, unchanged endpoints
and protected operations, direct original-to-representative comparisons and
post-target-quantization error validation. Do not choose numeric error defaults
or transfer OPLL24ms before listening evidence. Full pitch/LFO/bend, waveform
morphing and register/macro percussion remain separate backend work.

Architect review supported this ordering and highlighted non-universal frame
cadence, zero-volume ambiguity, inability to undo legacy fragmentation by an
inner-partition filter alone, cumulative approximation error and final-loop
ordering. No conversion performance or compression gain is claimed by this
design-only audit.
