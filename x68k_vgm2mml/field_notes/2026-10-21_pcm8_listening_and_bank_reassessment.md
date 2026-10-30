# PCM8 listening regression and bank reassessment — 2026-10-21

## User goal and scope of this audit

User attachment85b70f1a-4153-43a1-bf8a-391d2bdde41e reports generation and
MMDSP listening with best-effort, force, no-vgm and no-song-loop. The intended
improvement applies to all PDX-producing routes, including --psg-output pcm,
not just SegaPCM. Goal: practical reusable-sample MDX/PDX comparable in structure
to the supplied Galaxy Force and View Point references. FM binary-size work is
explicitly a later candidate. User asks for FieldNote reread before further
implementation, plus OPM clock correction check and existing PDX comparison.

Read project_knowledge and relevant PCM design, finite end, PCM8 production,
shared-clock, clock correction, Sega source/bridge/listening/capacity, force,
low/ultra_low, continuous PSG, full-song PSG capacity and bank milestone notes.
No production code changes, regression/test execution, compilation, input VGM
conversion or agent listening in this audit. Existing private artifacts were
read only; abstract diagnostics were written under ignored outputs.

## User outcomes

Generation: GFII10/10, TBLADE12/12, TBOUR6/8 =28/30. GFII01/02 are FM-only;
26 current published PDXs therefore agree with28 generated songs. One duplicated
GFII06 log line is not another song.

User reports FM+PCM audible: GFII03/09/10, TBL03/05/06/07, TBOUR02 (8 songs).
PCM buffer warning, FM audible: GFII04/05/06/07/08; TBL01/02/09/10/12;
TBOUR01/03 (12).
Track buffer warning, no playback: TBL04/11; TBOUR04/07/08 (5).
TBL08 listening outcome is not supplied; do not invent it.
TBOUR05/06 generation blocks: existing errors are MDX track-start offsets
>0xfffe, at track4/9 respectively. These are no longer timeout results.

This is user listening evidence, not an independently captured runtime trace.
Do not conflate publication success, source-bank reuse, loaded PCM, PCM buffer
fit and MDX command-track capacity.

## Existing target mode versus missing sample structure

PCM8 enable E8 and16-track output were adopted as common production behavior
on Oct10. PSG->PCM already enters that same typed target. The missing feature
is generally reusable per-source-voice assets/events before mixing, not simply
enabling PCM8 or adding more PDX slots. Existing native MSM6258 already separates
samples/playbacks; do not regress it or claim arbitrary demixing of its output.
Sega and continuous PSG still normally become long mixed chunks. Initial finite
Sega bank implementation is inadequate for these real-song source trajectories.

Counts falling approximately by half with --no-song-loop are not evidence of
bank reuse: reader traversal changed and current strategy JSONs still select mix.
All published generated slots have distinct payload bytes; this does not prove
no within-song event reuse, since long-note holds and L also reference assets.

## Concrete source-interpretation gaps

Existing CSV first-fallback inspection, saved in
outputs/diagnostics/pcm8_reassessment_20261021/first_fallback_gates.json:
- GFII04: zero_delta gate2 begins left/right0, delta0, then receives gain,
  cursor, rate and control changes before natural stop.
- TBL09: sample_loop gate0 begins left/right0, delta0; no gain writes during
  the approximately20ms gate. Whole-song fallback can be caused by inaudible
  initialization, not just an audible looping instrument.
- TBOUR02: zero_delta gate30 starts muted and contains114 low-address writes,
  114 high-address writes and114 controls, plus volume/rate changes.
- TBL01: first failed held-control gate includes five address-pair/control
  writes and three changes of each gain register.

An enable-to-stop RuntimeGate is not a musical attack or one sample use.
Do not interpret114 address writes as114 attacks. Preserve ordered effective
cursor/fraction, bank/ROM generation, loop behavior, natural/explicit end and
held trajectories in a separate derived representation. Classify boundaries
as continuation, explicit restart or unresolved. Inaudible output can be omitted
only after checking the entire interval; source cursor/loop/PSG noise/EG state
must still advance so later sound begins correctly. GFII04's entire gate cannot
be discarded just because its beginning is muted.

## Independent representative PDX numbers

Table lengths below are encoded bytes, not decoded audio memory. Representative
single-bank tables and payload ranges were independently re-read in Python;
all rows have768 table bytes and distinct populated payloads.

file | total bytes | unique payloads | mean sample bytes | max sample bytes
--- | ---: | ---: | ---: | ---:
GFS reference |69675|16|4306.7|17616
VP3A reference |52987|5|10443.8|21069
VP3B reference |26537|8|3221.1|11015
GFII03 generated |150784|3|50005.3|62592
GFII04 generated |1362412|22|61892.9|62620
TBOUR01 generated |332832|6|55344.0|62496
TBOUR02 generated |1516800|25|60641.3|62552
TBOUR03 generated |118334|2|58783.0|62524

References use short assets repeatedly on multiple tracks; current generated
material remains near8-second/~62.5KB chunks on P. Compare stored waveform
bytes, not only populated-slot counts. This supports changing extraction, but
is not causal proof of native buffer warnings: user heard PCM for TBOUR02's
1.52MB PDX while TBOUR03's118KB gave a warning. No uniform player threshold can
be inferred, and identity of the actual player-loaded copies is not measured.
Do not make environment/setup questioning a prerequisite for fixing generation.

All-file abstract CSV/JSON: outputs/diagnostics/pdx_comparison_20261021/.
Independent representative values and user outcomes:
outputs/diagnostics/pcm8_reassessment_20261021/{representative_independent.json,user_listening.json}.
Reference24 distinct PDXs and generated26 current manifest/hash-pinned PDXs
were inspected. During review two diagnostic mistakes were corrected: counting
all MDXs in a directory against each PDX, and assuming all references use one
bank. Match MDX header pdx_name and verify complete bank tables instead.
RAY01C is840134bytes,3 tables/2304bytes,156 populated slots (64/66/26).
This confirms the production96-slot single-bank scope is not a general PCM8
limit. Increasing bank count alone would not reduce stored waveform volume.
Empty-slot requests may be deliberate silence; do not call them corrupt
reference data merely because a generic audit labels them non-valid.

## TBL01 pitch check

Input trace/header and conversion report declare OPM4MHz; target is4MHz.
Current project_controls applies nominal KC/KF correction only for a different
source clock. Existing TBL01 report: correction applied=False, clamp0,
max clamp error0 cents, known pitch loss=False. Thus clock correction did not
transpose this output. No claim of independent source-to-MML pitch equivalence:
existing report explicitly says pitch reproduction unmeasured, and pitch pass
CSV was not retained in this run. Any remaining perceived shift needs separate
note/detune/voice or acoustic evidence, not an assumed source-clock mismatch.

## Next design constraints, not implemented support

1. Establish per-voice sounding/continuation interpretation on concrete Sega
   cases, including muted initialization and live address reprogramming.
2. Extract common reusable-asset/playback/capability boundaries alongside the
   existing MSM6258 implementation; retain source-specific state semantics.
   Reuse existing typed P-W allocation/binding, not a new universal PCM rewrite.
3. Extend bank conversion without discarding offsets, phase or internal loops.
   The ordinary PDX table stores offset/length, not native source loop points.
   An MDX note repeat is not automatically equivalent to an uninterrupted
   Sega loop. Encoded ADPCM mid-slices also need predictor/nibble evidence or
   independent re-encoding, not an arbitrary pointer splice.
4. Apply common target machinery to PSG/Delta/native MSM as feasible. PSG noise,
   shared EG and oscillator phase must not reset at inferred note boundaries;
   the prior explicit continuous-source decision remains valid.
5. Fallback may eventually be localized, but banked and mixed source sets must
   be disjoint, preserve boundary sound and share the8-track budget. Merely
   rendering a full mix alongside banked voices would double those voices.

The goal is reusable-bank behavior wherever source evidence permits it, not
promising small banks for arbitrary already-mixed MSM or evolving PSG noise.
Native buffer load, final MDX size and approximation quality need separate
acceptance metrics. User-owned local_only regression/listening remains the final
practical check. No commits/staging/push or unrelated mucom/enhancer work.
