# Scoped OPNA --force checkpoint (2026-10-15)

## User decision

Implement --force before extending bidirectional FM mapping policies.
Explicitly allow known unsupported source functionality to be omitted in target
projection, preserving source IR, time and human-readable loss records.
First scope is OPNA SSG noise/hardware-envelope intervals. User owns private
regression and listening; do not implicitly resume OPM-to-OPNA implementation.

## Reported source checks

gh011 exported successfully. Read-only inspection shows 16 independent playback
segments, five PDX samples, and continuous_decoder_epochs=false. It did not
exercise the new held-DELTA control reconstruction route.

gh002 failed at PSG ch0 row11 noise/hardware EG not implemented. That first
blocker is SSG target projection, not evidence of a DELTA decoding defect.
No private inputs were reconverted during this task.

## Implementation

Canonical CLI and export_mdx accept --force, scoped to OPNA->MDX. Source SSG
tuples remain immutable. Selected unsupported intervals are copied into a muted
target view (mode/volume/envelope disabled); ordinary tone intervals retain
normal inferred attack/release processing. Tone+noise loses the entire interval,
rather than inventing a noise-free substitute.

The original SSG CSV is saved even without --dump-passes when forced. Per-row
CSV/JSON records include source channel/event IDs, source row, loop iteration,
source sample interval, target channel and separate noise/envelope reasons.
force_projection is retained in OPNA projection and canonical conversion JSON.
TXT summarizes counts and up to eight intervals; full diagnostics stay under
the listening export _diagnostics tree. Source elapsed time is unchanged;
omitted_channel_samples sums per channel and may overlap in song time.

CSM, CH3 special frequencies, unknown FM effects, unexpanded streams, malformed
sources, PCM limitations/capacity and compiler limits still reject. PCM strict/
best-effort policy is independent. No change to target clocks or normalization.

## Focused verification

Saved worktree changes before verification. Three authored tests passed:
source invariance, tone/noise/tone/envelope/tone release/restart edges and CSV
serialization; CSM still rejected with force; forced success followed by strict
rejection clears the prior omission reports and MML.

One authored canonical CLI conversion with --force --dump-passes generated MML
and diagnostics under outputs/opna_force_check. Two omitted intervals,
source end17740 samples. Original SSG envelope survived in source CSV; both
corresponding target performance intervals were silent. TXT loss summary and
MML were inspected. No MDX compiler, private regression or listening was run.

Initial smoke caught nonuniform target CSV columns on omitted rows; all target
rows now carry the same diagnostic columns, and dump checks were added.

Working-tree changes only. No staging, commit, push or branch changes.
Focused review found stale omission reports survived a rejected strict retry.
Added cleanup for those owned diagnostics to converter, frontend and exporter;
the new authored retry check passed. No other actionable issues were found.
User can retry gh002 with --force alongside
existing --pcm-policy best-effort and --compiler mmlx.
