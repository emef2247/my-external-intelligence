# BURNFC03: initial native capture budget rejects valid polyphony

User supplied an incomplete BURNFC regression log and requested a focused
investigation of BURNFC03. This request authorizes source inspection and one
focused conversion experiment for BURNFC03; it does not authorize a new catalog
regression. Existing user output was not overwritten.

## Confirmed failure
Existing .c140_projection.json reports total_resource_limit at gate2400.
Native source parsing and State/Segment dumps complete. The rejection is in
c140_bank.render_gates before waveform deduplication, target 96-slot reduction,
PCM8 allocation or packed PDX size checks. It is an implementation resource
budget, not a C140 hardware limit or PDX/EX-PDX file format restriction.

Current cap is32,000,000 mono native frames across all keyed voices. Source
clock12288000 gives42666 native frames/second. One pass is71.102131519s. An
algebraic frame/cursor accounting diagnostic predicts35,058,325 captured frames
(109.56% of budget,70,116,650 bytes of int16 wave arrays before ancillary data).
This diagnostic performs no ROM reads/decoding and is not a fidelity comparison.
It respects fractional cursor, natural stops and internal-loop wrapping for this
source's delta<=65536 case. It reproduces the exact error gate2400, channel9:
interval64.640978765..64.648924202s, cumulative31,999,943 frames, next339 frames.

Source uses16 channels, average captured voices11.5565,2682 keyed gates. Two
internal-loop voices on channels6/7 persist37.854263s each, consuming3,230,180
frames together. Longest gate is1,615,090 frames, below the independent4m gate
cap. Zero-frequency pauses total6777 frames; both-gains-zero intervals66993:
removing these alone cannot solve the aggregate excess and must not fake KeyOff.

## Experiment and interpretation
An in-process-only48m aggregate budget runs the existing C140 converter with
force=True,song_loop=False,dump_passes=True, standard PCM profile. No source,
conversion code, or default value was modified. User output remains untouched.
Outputs and the experiment result are under
outputs/diagnostics/c140_burnfc03_20261025/expanded_budget.

The budget is applied to every observed voice frame before reusable assets are
selected, so repeated samples still consume source-capture work. Raising this
cap cannot itself guarantee final96 slots, target8 voices,1.2MB PDX or runtime
playback-memory fit. Initial corrective direction: expose a bounded capture
budget or use bounded incremental capture/reuse; do not turn off safety limits
or silently discard necessary long looping voices. Exact experiment outcome
will be appended after completion. No other reported BURNFC failures were
investigated in this focused task.

## Completed experiment
Canonical convert completed in596.997s with the temporary48m cap. Native bank
produced838 samples/2682 playbacks,2,984,063 encoded candidate bytes. Shared
budget96 reduction and force target allocation retained80 samples/1988 playbacks.
Final EX-PDX808873 bytes; MDX49978 bytes. All694 final omissions are explicitly
"eight PCM tracks already active" in .pcm_omitted_playbacks.csv;80 bindings
were inspected. This is a canonical converter diagnostic, not a fresh batch
mmlx regression, emulator playback or listening check. Native/target artifacts,
assessment and final omission reasons were inspected. Default32m remains
unchanged. No conversion code changed, so no new automated test run was needed.
Japanese ignored analysis: outputs/diagnostics/c140_burnfc03_20261025/analysis_ja.md.

## User playback feedback and default update
User reports MXDRV track-buffer shortage when loading the generated BURNFC03,
and explicitly asks to retain the relaxed conversion limit nonetheless.
MAX_TOTAL_FRAMES is now48,000,000 by default. Per-gate4m, optional-completion32m,
target sample and PDX byte budgets are unchanged. This fixes the earlier
aggregate capture rejection only. The playback error is a separate unresolved
observation; no cause inferred from the message alone. Documentation/handoff
updated. No private conversion rerun or README edit.
