# PSG user regression: missing accepted-clock entries (2026-10-16)

User asked to inspect DSLAYER4 PSG -> MUC errors first, distinguishing chip
mapping capability from missing MUC implementation. No fix, conversion, tests,
compiler or listening performed. Read existing results/manifest/timing/source
PSG JSON and implementation only.

## Existing result

20 songs,12 success and8 conversion_failed: DSLY4_04/05/07/12/14/16/18/19.
All8 stop at musical_structure_and_rendering with held SSG gate crossing the
song-loop point. They show0force omissions. This is not a reported unsupported
noise/envelope or PCM capacity error.

Every failed manifest has normalization.applied=True and rejected SSG seam
proof reason `SSG loop write lacks a recorded target clock`. Some channels
never reach further source-state/phase checks because this first condition fails.

Missing source control times inside loop (distinct samples):
04:404,05:22,07:324,12:26,14:1639,16:6,18:199,19:196.
They include ordinary tone/mixer/noise writes, not merely I/O events.
Existing source PSG JSON retains all actual writes; accepted timing map lacks
these coordinates. No source raw payload copied into public documentation.

## Code path

mucom_normalization.normalize_source builds times from native OPNA analysis
(events/unmodeled writes), PCM controls and already merged musical units/slices.
The new external PSG source is retained separately; its real0xA0 timestamps
are not supplied to this normalized allocator. Redundant/inactive or merged
PSG controls therefore lose clock coordinates. Baseline choose_clock already
receives all raw command times, but normalized selection supersedes that path.

analyze_ssg_seams correctly uses0xA0 source writes after the prior patch.
opna_ssg_loop.prove_loop_states needs every observed loop write timestamp in the
accepted map. It rejects on the missing coordinate; ssg_loop_seams stays empty.
render_track already implements split_unit_at_loop plus L/tie and restoration
for proven stable_carry/observed_head_restore. Without a proved seam it raises
the final held-gate error. Thus this is an integration defect in normalized
source-time propagation, not wholly unimplemented SSG loop MUC spelling.

## Proposed next implementation (not performed)

Pass external PSG observed-control timestamps into the same ordered/bounded
normalization allocation, retaining source IDs and control/trigger semantics.
Use the accepted single map for rendering and loop proof; preserve reg13 trigger
order and any required positive side-effect intervals. Never fabricate OPNA
source events or fill missing coordinates after allocation with an unchecked
nearest-clock guess. Keep native IR unchanged and dump the added timing evidence.

Then rerun only a tiny authored normalized-loop fixture with redundant/inactive
PSG writes. User owns private reruns. A second source-state/restoration/EG phase
condition might surface after this first defect is fixed; do not promise all8
songs will convert solely from this diagnosis. No guard bypass/loop disabling
or broader chip projection change was made.
