# PCM auto, mixed direct/DAC supply and verification VGM opt-in

## Latest user regression: same status, new blocker

Read saved Nemesi95_STATE24 and Sion_STATE24 results only. NEME9501/03/04/15/18/25
and SIONII01-08 now have no native source issues. All14 stop at playback0
irregular_byte_supply. All14 mix direct B7 and derived DAC supply. The old
fallback required stream byte count == all supplied bytes, so it excluded
these otherwise captured samples. No private input was reconverted or compiled.
Saved summary: outputs/diagnostics/msm6258_mixed_delivery_20261023/
existing_failure_summary.csv. Do not mistake this new blocker for failure to
apply the preceding pan0x88/0x80, register0x17 and stopped-data corrections.

## Implementation and steering

Continuous-PDX best-effort fallback now accepts direct, stream or mixed capture
with only irregular supply, known reset/pan, a nonempty sample and consistent
stream counts. Native cadence/eligibility flags and raw byte history unchanged.
Strict rejects known delivery timing loss. Target assessment includes direct
and stream byte counts, cadence error and unsupplied PLAY tail. Shared guard
covers single/parallel bank preflight and PCM rate/size profiles. Target asset
length can differ after explicit reencoding/replacement; original supplied-byte
count is source evidence and must not be rewritten for target convenience.

User requested auto default: actual DAC supplies -> best-effort, otherwise
existing strict/direct route. Mixed supply counts as DAC. Unused declarations
and stream setup do not trigger best-effort. Explicit policies override auto.
No source issue is concealed. Source/target choices and reason stored in
assessment policy_selection, batch CSV, console and TXT report. Other source
projections without DAC supplies retain strict behavior by default.

User also requested --no-vgm become unnecessary. Batch API/CLI now omit
verification VGM by default; --vgm requests MDX replay. Legacy --no-vgm remains.
MML/MDX/required PDX remain default products; internal projected VGM evidence
is not removed. Existing replay tests opt in explicitly.

## Validation and limits

New tests cover default/direct, unused DAC setup, mixed origin, explicit
strict/best-effort, unknown reset rejection, size profiles, both PCM allocation
paths, default replay exclusion and --vgm opt-in. Authored mixed/default auto
produces MDX+774B PDX and no verification VGM; authored direct chooses strict
and generates MDX+896B PDX; --vgm on authored DCSG emits VGM. Relevant final
counts and policy/asset paths are in verification.json in the same output folder.
Architect reviewed and found no material remaining issue in scope. Earlier
quota-limited review attempts are not counted as completed reviews.

Successful best-effort conversion is not supply-timing/acoustic equivalence.
Local_only/listening remains with user. No staging/commit/push and no MUCOM/MSX
changes this task. Preserve all previous staged/unrelated edits and notes.
