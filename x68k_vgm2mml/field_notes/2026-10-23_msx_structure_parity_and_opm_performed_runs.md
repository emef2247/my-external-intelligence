# MSX structure parity and canonical OPM held runs (2026-10-23)

## Authorized scope

User requested auditing whether proven MSX structure/cleanup is used by x68k,
adding missing processing and verifying only public inputs. User owns local_only
regression/listening. Existing EX-PDX edits from the preceding task remain in
the public working tree; no stage/commit/push performed.

## Audit findings

Reviewed the MSX FieldNotes on sample note normalization, its benchmark,
continuous-note inner-loop recovery, pre-envelope source loops and MDX target
compaction; also x68k2026-10-11 short-gate/clock decisions. Relevant code:
opll_note_units.group_notes, note_normalization, source_loop_plan,
loop_structure, pre_envelope_loops, musical_structure, opm_mdx_music,
opm_loops and mdx_compaction.

The major structural core was already shared: source_loop_plan/loop_structure,
opll_note_units/opll_inner_loops, pre_envelope_loops and mml_envelopes are
identical after newline normalization. OPM source loops match; MSX's source_plan
optimization is in x68k musical_structure. x68k MDX compaction has additional
relative-setting and duration reductions. There was no missing wholesale
hierarchical-loop solver to transplant.

The concrete gap was positive performed-state canonicalization: MSX groups
adjacent equal positive states independently of harmless Segment partitioning.
OPM built an additional tied note slice at each surviving write tick, even if
same-tick KC/KF changes restored the previous positive state. Its outer unit
key also retained the full zero-time raw history. Thus target-equivalent
performed notes could have extra slices or fail the source nomination key.

The MSX OPLL<=44samples omission is a distinct approximate rule. It was NOT
ported as general OPM state deletion: OPM phase/operator/global side effects
differ. The existing8ms complete-gate/timing-bound policy remains unchanged.
This task adds exact grouping after projection, not another fidelity loss.

## Implementation

New opm_performed_runs.HeldRun/coalesce groups adjacent, identical complete
projected OPM states only inside one ordinary complete attack. A new raw action
or pending command is a barrier. Source IDs and native unit trajectories are
retained; helper event aggregation is linear rather than repeated tuple append.

opm_mdx_music reuses unchanged uniform carrier volume only while known; a raw
write invalidates the cached setting. Raw operator/pan/global operations keep
order. Short positive different-pitch/state runs remain. Retriggers, partial
keys, gaps, release and song-loop boundaries are not crossed. Long notes still
use the existing<=256tick splitter and ties.

Inner keys use canonical positive runs. Outer performed_key adds onset state,
emitted entry actions, positive state runs, pending terminal actions, release
writes and final state. Different terminal state/actions cannot be silently
substituted. The shared planner still validates exact expanded rendered tokens.
Raw fallback/control units remain conservative. Native State/Segment/PCMIR not
changed, no arbitrary y-command removal and no SW-envelope syntax invented.

New .mdx.structure.performed_runs.csv records source interval->target run
membership, source IDs, state and ordered actions. Unit CSV retains original
trajectory and adds performed_key. Existing plain/uncompacted/final MML and
inner/outer loop diagnostics remain. Summaries add performed/merged run counts.
State JSON is serialized only on dump, avoiding duplicate large state strings
in ordinary conversions.

## Verification

133 distinct focused checks passed: musical renderer12, compaction17,
OPM normalization7, PSG/SCC target9, duration limits3, duration replay4,
PSG/SCC-to-MDX1, PCM projection46, song-loop6, target pruning12,
source-loop planner4, new performed-run tests12. Four existing duration-replay
tests initially failed because their IdentitySamples test double lacked the
source_clock_hz required by the current comparator. Added4MHz to the test double
(the reference is already a replay at target clock), then all4 passed. No
production behavior was weakened to satisfy those tests.

Public OPM directory contains43 inputs.42 supported inputs compiled/replayed
and passed source/target checks. The remaining from_psg/block_boundary input
contains active OPM+SCC and correctly stops at existing source preflight before
structure construction. No supported-case state mismatch found. The existing
public cohort had0 merge candidates; its result establishes no regression,
not a measured size reduction for those songs.

New authored coverage includes distinct positive state, raw operator/pan/LFO
barriers, exact raw-action order, repeated key edges, different terminal
controls, source membership/dumps and rawalgorithm changes followed by volume
restoration. A same-tick changedKF+restore case has different native histories
but equal positive state trajectories and now equal performed keys.

Authored8note comparison isolates coalesce against its identity variant using
the same new renderer:12inputruns,4merged; MML538->519chars; MDX218->212bytes.
Real mmlx compile/VGM replay has identical positive full-state intervals and
times on all8channels after joining only adjacent equal replay states. This
is state/timing validation, not waveform/native hardware/listening proof.
No promise that KNIGHT or other private overflows now fit.

Artifacts:
outputs/diagnostics/structure_parity_20261023/focused_checks.json
outputs/diagnostics/structure_parity_20261023/public_opm/{results.csv,inspection.json}
outputs/diagnostics/structure_parity_20261023/authored_compare/inspection.json
and baseline/grouped MML,MDX,VGM and structure/pass CSVs in that folder.

## Documents / remaining scope

Focused public design:docs/opm_performed_structure.md, linked from
opm_note_lengths.md. No project_knowledge FinalRule edits. Architect prereview
selected exact grouping before approximation and final review found no material
blocker. git diff --check passed. User runs local_only regression next.

Further reductions may need scoped redundant raw controls or explicit
approximation policies. Do not introduce broad1ms/8ms state removal, change
source Segments or claim MGSDRV macro syntax reduces compiled MDX automatically.
Prior EX-PDX/PDX-cap behavior remains saved. Prior bank resource/ROM/internal-loop
work is separate and not claimed complete. External note is in a separate
private repository and is not part of a public project commit.
