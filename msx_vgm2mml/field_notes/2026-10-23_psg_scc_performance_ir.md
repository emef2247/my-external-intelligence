# PSG/SCC performance IR: first separation milestone

Started2026-10-23; verified2026-10-24. The user supplied and explicitly authorized
59 PSG/PSG+SCC local inputs for agent conversion/comparison. OPLL/MSLND is excluded.
The parent gra2_002 VGM substitutes for the empty sync-mark VGM folder as approved.
No input changes, staging, commits or pushes.

## Representation and consumption

New py/psg_scc_performance.py retains immutable native Segment references/order,
source-audibility spans, typed ordered per-Segment control snapshots and inferred
renderer note partitions with explicit boundary reasons. Controls cover pitch,
volume, SCC timbre/enable and PSG noise/mixer/hardware-envelope state/reset.
Original44100Hz source sample coordinates and60Hz renderer ticks stay separate;
unknown coordinates stay unknown. Zero-time controls and same-value resets remain.

Existing conservative extract_notes policy is retained exactly: pitch writes,
settings changes, volume rises, envelope resets, rests and gaps determine renderer
partitions. This is not completed vibrato/portamento or score-attack recognition.
SoundGate is an interpretation of audibility, never an observed source Key-On.

EnvelopeBank.submit builds one IR. LoopFirstEnvelopeBank consumes its immutable
relative structural keys and complete units before selecting shared envelopes;
render consumes the same IR through a mutable legacy Note adapter. Target envelope
assignments do not alter note identity. Structural equality excludes source IDs,
absolute times and diagnostic gate IDs. Source state/Segments and SW curves are
preserved. --dump-passes emits performance JSON plus notes/gates/controls/
boundaries/segments CSVs. Failed reruns remove these known generated artifacts.

## Validation

- Public PSG13 + SCC13 + PSG_SCC2:28/28 MML byte-identical to the preceding
  frontend baseline; all native MGSC compiles succeed.
- Authorized local59:59 MML conversions succeed; all source SHA-256 values match.
  All MML outputs and5,048 existing CSV/MML artifacts match the pre-IR renderer
  byte for byte. Baseline freezes HEAD mml_envelopes/pre_envelope_loops in a
  current frontend snapshot; options are identical. Validation flags are
  --vgmticks --dump-passes with normalization disabled to isolate this refactor.
- Native MGSC55/59 succeeds. DSLY4_02, DSLY4_11, DSLY4_14 and
  gra2_016_with_sync_mark03 fail with Track buffer full. Each failure reproduces
  with the identical baseline MML; no new emitted-MML regression is identified.
- Focused66 checks:65 pass and1 pre-existing allocation expectation mismatch
  (15000 in the assertion versus committed16000). Additional3 CLI checks pass,
  including removal of all12 PSG/SCC performance artifacts after a failed rerun.
-14 authored IR tests and pipeline identity/timeline test pass. Reviewer found
  no blocking semantic/integration issue; completion scope must not claim new
  score inference or audio equivalence.

Local aggregate: PSG291,294 Segments /5,446 audibility spans /71,049 renderer
partitions /1,165,176 typed control snapshots. SCC272,089 Segments /12,057 spans /
17,400 partitions /1,088,356 snapshots. These are state snapshots, not raw register
write counts. All retained source sample coordinates are known in this set.

SCC3,755 partitions span multiple source-audibility gates because legacy rendering
can ignore positive-sample silence rounded below a renderer tick. PSG flagged0.
The new gate_ambiguity diagnostic exposes this inherited distinction; the change
does not resolve it or invent attacks. Future score inference must evaluate these
boundaries explicitly. MML equality establishes compatibility, not musical truth.

## Storage recovery and evidence

I: became full during conversion. Only this task's generated diagnostic directory
was moved to Windows TEMP, and all incomplete conversions were rerun successfully.
New performance/public output trees and aggregate reports are restored under:
outputs/diagnostics/psg_scc_curves_20261023/.
Baseline artifacts remain at:
C:/Users/ef110/AppData/Local/Temp/msx_psg_scc_curves_20261023/baseline.
verification.json/md and results.csv record the final results and locations.
No user fixture or pre-existing regression output was removed.

Next: user listening/expectation review and a separately justified attack/continuous-
pitch interpretation. Do not flatten PSG/SCC SW envelopes, treat audibility as
source Key-On, substitute ticks for unknown sample timing, or claim raw-VGM replay.
