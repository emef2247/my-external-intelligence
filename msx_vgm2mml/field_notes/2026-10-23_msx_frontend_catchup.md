# Active checkpoint: MGSDRV frontend catch-up (2026-10-23)

Implementation is saved in the working tree. No staging, commit or push.
Existing unrelated README copies/public fixture directories are untouched.
The earlier shared helper/note-normalization edits are retained and extended.

## Current behavior

- Shared immutable MgsOptions feeds the converter and batch. With no normalization
  switches, conventional MSX output remains unchanged. Explicit --fm-state-ms or
  --normalization-ms enables optional normalization; rejected clock/projection
  keeps the ordinary output and records unchanged/reason.
- MSX effective defaults: FM1ms/44samples, correction8ms/352samples. X68000's24ms
  default is not assumed to be OPLL fidelity evidence. FM filtering is confined
  to OPLL melody inside accepted normalization. No independent PSG/SCC clock
  estimator, no whole-gate deletion and no PSG/SCC envelope filtering added.
- Release-final states and both sides of voice/custom-patch/sustain changes are
  protected. Rhythm boundaries obey the correction budget; newly merged distinct
  attacks reject normalization atomically. Original traces/Segments stay native.
- Force uses a validated finite command inventory: known OPM/OPNA/DCSG/MSM6258/
  SegaPCM/second-OPLL writes may be omitted, with exact source offsets/times/data.
  Unsupported-only input, malformed VGM, unsafe decoder variants and PCM stream/
  transfer commands still fail. Mere header clocks and existing undeclared muted
  SCC initialization are separate from forced omissions.
- Batch default timeout1200s. Primary success/conversion_failed/compilation_failed
  with stage/error_code/detail and legacy_status; incremental CSV/JSON plus raw
  logs and per-input report JSON/TXT. Nonempty current-run MML and MGS required.
  Key-on comparison remains independent, with its previous enabled default.
- New conversion, source inventory, force and normalization/FM decision reports
  survive failure. Known stale generated files are removed on rerun; unrelated
  user files/directories are not removed.

## Verification and limits

Public60 inputs:56 compile,4 existing MGSC track-buffer failures (cpebach,
enchanted,turpinh,bridge). Their MML is byte-identical to HEAD frontend output
using unchanged renderers with normalization disabled; HEAD compile also fails.
Authored STATE_PROBE:1ms retains all state runs,24ms omits24 internal pitch states;
both normalize and compile, retain24 attacks and identical native Segment hashes.
Public sample retains the historical applied oracle at explicit16.6667ms budget;
new8ms default may safely abstain. Segment evidence stays identical.

Related test-suite result is recorded in outputs/diagnostics/msx_catchup_20261023/
focused_tests.json/log. The allocation assertion expecting15000 versus committed
16000 is pre-existing and is not edited; HEAD/current behavior is identical.

Existing libkss roundtrip check is NOT validated as passing:18/19 public OPLL
exports hit its duration guard, one compares19 source Key-Ons versus0 output.
That one has byte-identical HEAD/current MML and MGS and the same HEAD mismatch.
No blanket playback-equivalence claim. The export/runtime cause is not resolved
by this frontend catch-up. --skip-keyon-counts is available for compile-only runs.

All evidence: outputs/diagnostics/msx_catchup_20261023/.
Docs: docs/note_normalization.md, docs/mgs_force.md, docs/batch_mgs.md.
External private note: field_notes/2026-10-23_msx_frontend_catchup.md.

## Next allowed action

User runs local_only threshold sweeps/listening and may commit this checkpoint.
Stage3 (separate one sounding note from typed pitch/volume/timbre curves) has NOT
started. The user will provide its test patterns; wait for those before starting.
Do not blanket-prune PSG/SCC SW-envelope curves, relax compiler limits silently,
claim that a requested threshold was applied without inspecting its status, or
change unrelated allocation expectations to make this suite green.


Final focused suite:117 checks,116 pass,1 confirmed pre-existing allocation assertion.
See outputs/diagnostics/msx_catchup_20261023/verification.md for final evidence.

## Export entry follow-up (2026-10-23)

scripts/export_mgs.py delegates to the existing batch runner, with explicit
FM-state24ms / normalization8ms defaults and CLI overrides. No batch source or
shared MgsOptions defaults were changed in this follow-up. Numeric defaults
request optional normalization; unchanged/reason remains visible when rejected.
Other batch behavior, including optional key-on checking, is inherited.
Four focused export checks pass, including a public PSG conversion. Evidence:
outputs/diagnostics/export_mgs_20261023/. Stage3 remains awaiting user patterns.

