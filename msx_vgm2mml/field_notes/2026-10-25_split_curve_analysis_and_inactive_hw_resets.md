> Current production status: inactive-reset coalescing IS integrated under
> --software-volume-curves; --no-software-volume-curves selects the ordinary path.
> The default remains off. All six authorized split cases compiled. Sections
> describing diagnostic-only status are historical; see the production restart
> record below and2026-10-25_curve_integration_history_and_regression.md.

# Split-fixture sounding units, loops and software envelopes

Date: 2026-10-25

## Purpose and scope

This is the English knowledge record of the local Japanese analysis
`outputs/diagnostics/psg_scc_curves_impl_20261025/analysis_ja.md`, expanded with
an audit of the previously adopted candidate-selection policies. The user
backs up outputs separately; this note preserves the conclusions independently
of those untracked artifacts.

The six GRA2_001 split VGMs are deliberately small reproductions of the
whole-song buffer problem. Use the six VGMs and matching/master MML materials
under tests/fixtures/local_only/psg_scc/msxplay.com/gra2_msx/gra2_001/
with_sync_mark/. Recover sounding units, loops and envelopes here before
scaling up. Do not substitute unrelated NEMESIS logs or launch another full-song
regression. The primary metric is each channel's MGSC allocated/used/free buffer,
not padded MGS file size, MML characters, loop count or envelope-ID count.

## Reference provenance and corrected comparison

The current folder contains six VGM/MGS files but 98 numbered MML fine cuts.
Current same-name01..06 MML durations are64,742,10,10,10,10 frames, unlike the
six VGMs at approximately889,1284,964,1925,1925,1564 frames. Earlier direct
same-name MML size/envelope comparisons are withdrawn. The 2026-10-03 inventory
recorded a different folder state after regeneration; do not infer that today's
same-name files have the same relationship from that historical inventory.

All six existing MGS regenerate the source VGM byte-for-byte. The user approved
`gra2_001_with_sync_mark.mml` as the reference master for interval mapping.
Its compiled music block matches the parent reference MGS, and the six existing
MGS definition streams match the master. Master source windows are:

| Split | Master score frames [start, end) |
| --- | --- |
| 01 | 0, 886 |
| 02 | 886, 2166 |
| 03 | 2166, 3126 |
| 04 | 3126, 5046 |
| 05 | 5046, 6966 |
| 06 | 6966, 8526 |

All 222 master per-track synchronization annotations agree with the computed
timeline. The boundaries are declared synchronization points and do not cut
existing top-level loops. Captured VGM setup/tail overhead is approximately
3.1..4.2 frames; it was reported rather than silently trimmed. PSG3 ordered
pitch transitions match all six corresponding reference windows. This does
not establish arbitrary all-register, phase or audio identity across windows.

Reference used bytes come from matching existing MGS offset spans, not from
recompiling unrelated same-name MML. The offset-span method was independently
validated against MGSC -T for all nine master tracks.

## Concrete PSG structural defect

`py/psg_scc_performance.py::_notes` treats every native PSG evS/global hardware
envelope reset as a renderer-note boundary, even when that channel's
`envelope_enabled` is false. PSG1 and PSG3 never enable hardware envelopes in
these six cases. Operations intended for the hardware-envelope channel2 split
unrelated fixed-volume channels into artificial sounding units.

Pure reset-only boundaries total386 on PSG1 and385 on PSG3: 771 in all.
Per-case counts are3/3,80/80,57/57,126/125,72/72,48/48. These boundaries obstruct
repeated score/phrase recognition before envelope allocation even starts.

Native reset and phase evidence must remain intact: a global reset can matter
to a later hardware-enable operation. The proposed correction belongs in a
derived performance/target-note view, with original Segments and ordered
controls retained. Active hardware resets must remain protected.

### Diagnostic causal check, not production behavior

A diagnostic-only pass joins neighboring derived ScoreNotes only when the right
boundary reason is exactly hardware_envelope_reset, both sides are sounding
fixed-volume PSG, settings match, time is contiguous and the same single source
audibility gate spans them. It retains source membership, source Segments and
controls. The resulting real MGSC used bytes are:

| Split | PSG1 ordinary -> diagnostic join | PSG3 ordinary -> diagnostic join |
| --- | ---: | ---: |
| 01 | 197 -> 184 (-6.6%) | 160 -> 150 (-6.2%) |
| 02 | 599 -> 238 (-60.3%) | 596 -> 254 (-57.4%) |
| 03 | 596 -> 263 (-55.9%) | 591 -> 258 (-56.3%) |
| 04 | 710 -> 343 (-51.7%) | 953 -> 552 (-42.1%) |
| 05 | 1112 -> 421 (-62.1%) | 1100 -> 458 (-58.4%) |
| 06 | 1295 -> 429 (-66.9%) | 930 -> 490 (-47.3%) |

These are counterfactual compilation measurements. The reset join has NOT been
integrated into production and has NOT completed full replay validation.
It must not be advertised as an effect of --software-volume-curves yet.

## PSG loop structure

NOTE counts below expand macros at each call but count finite loop bodies once;
rests are excluded. They are source-syntax metrics, not compiled byte counts.
Generated curves here mean the current production opt-in curve stage, without
the diagnostic inactive-reset join above.

| Split | Reference-window NOTE | Ordinary generated NOTE | Curve generated NOTE | Reference / curve physical loops |
| --- | ---: | ---: | ---: | ---: |
| 01 | 98 | 128 | 101 | 0 / 4 |
| 02 | 282 | 599 | 599 | 6 / 38 |
| 03 | 271 | 436 | 436 | 1 / 22 |
| 04 | 92 | 476 | 476 | 3 / 23 |
| 05 | 201 | 820 | 820 | 6 / 11 |
| 06 | 172 | 627 | 627 | 13 / 31 |

More short loops do not establish recovery of the original sounding units or
phrases. Correct irrelevant boundaries before interpreting loop count as success.

## Envelope coverage and channel-buffer effects

The master has27 definitions; generated output fills32 slots including constant
volume. Original PSG also uses software envelopes; they are not SCC-only.
These definition counts are not a recovery rate. Per-channel/window bindings
and nominal durations are in master_window_audit.csv and envelope_coverage.json;
no one-to-one source-shape recovery percentage is established.

The current curve stage joins24 PSG and198 SCC boundaries. It also changes
shared-bank competition: case04 SCC1 grows256 bytes; case05 SCC1/2/3 grow
60/396/401 bytes. Long complete candidates displace previously selected curves,
which then fall back to inline volume commands. Joining more notes does not
by itself guarantee smaller buffers, and a whole-song bank can behave
differently from isolated excerpt banks. No whole-song buffer fix is claimed.

## Historical selection policies: mandatory starting point

Re-read the following records rather than restarting the selection debate:

- 2026-09-29_gra2_compression.md: complete sounding units before loops/envelopes;
  shared selection independent of chip submission order. A byte-benefit allocator
  was proposed there, not demonstrated as the final adopted selection policy.
- 2026-09-29_macro_compression.md and 2026-09-30_nemesis_macro_capacity.md:
  tested macros save source characters but real-song compiled track bytes were
  unchanged. Macros alone are not a buffer-full remedy. Sync-boundary expansion
  and actual per-channel allocation matter separately.
- 2026-10-03_psg_scc_pre_envelope_structure.md and
  2026-10-03_gra2_msxplay_structure_comparison.md: reversible loops over complete
  notes precede the shared bank; count stored structural representatives rather
  than every expanded occurrence. Preserve all original rendered occurrences.
  Longer observed curves retain priority. More source markers or fewer text
  characters do not guarantee smaller compiled tracks.
- 2026-10-03_structured_macro_experiment.md and the current
  docs/structured_macros.md: compare complete bounded allocation alternatives,
  including the unchanged baseline; exact expanded timed-command equality is
  mandatory. Wider search is not necessarily better. Current choices remain
  bounded and partly greedy, not globally optimal.

Current docs/software_envelopes.md and py/mml_envelopes.py confirm the adopted
user preference: rank varying observed curves by descending duration, then run
count, with deterministic ties. Reserve constant ID0;32 IDs total; reject new
encoded data longer than220 characters. A single-use curve with fewer than
three runs is ineligible for its own definition but may share an already
selected longer curve's exact observed prefix. Prefix equality covers every
observed tick; no unseen tail or base-volume family is inferred. Unselected
curves retain explicit tied volume changes rather than being flattened.

Therefore, reconsider selection starting from the existing pipeline:
complete sounding units -> reversible repeat structure -> stored representative
counts -> shared long-curve-priority envelope bank -> state-safe rendering and
macros. Do not replace the established long-curve preference with a new global
byte-saving policy without explicitly identifying that policy change.

The first controlled comparison should integrate irrelevant-reset coalescing
while HOLDING the existing envelope/macro selection policies fixed. Then
separately inspect displaced definitions and each affected channel's actual
compiled usage before considering any further candidate-selection change.

## Flag status at the original analysis checkpoint (superseded below)

The flag is available in vgm2mml.py, scripts/export_mgs.py and
scripts/batch_vgm_to_mgs.py, disabled by default. It joins compatible fixed-pitch
volume-only partitions into supported single rise-and-decay curves before loop
planning, shared selection and rendering. It does not smooth away volume holds.
Native Segments, controls and the original PerformanceIR are preserved.

It still protects every hardware_envelope_reset boundary. Thus it does NOT
apply the diagnostic PSG improvement measured above. It also does not implement
a new envelope allocator or a new macro-selection policy.

Final target tie compensation is integrated under the flag for validated uniform
255-score-step schedules at tempos75/225. Actual MGSDRV playback consumes an
additional software-envelope update at a tied continuation; encoded hold padding
compensates for these boundaries without changing note/loop/macro bodies or
allocating new envelope IDs. Unsupported schedules, accepted normalization and
oversized definitions rebuild the complete ordinary pipeline. Check final
<stem>.volume_curves.target.json and chip summary applied fields; planned
candidate joins must not be mistaken for applied output when fallback occurs.

104 focused tests previously passed, including13 real MGSC/libkss module tests
covering PSG/SCC, tempos75/225, shared300-frame curves, exact85/255-frame prefixes,
short release and rest/restart. Curve values match from actual onset and
note/rest masks match the same uncompensated score. Existing raw75 c%255 release
overhang and one-frame next-onset delay remain a separate driver behavior.
Six corrected target MMLs compile and export VGM; no universal replay claim.

## Saved artifacts and next allowed work

Outputs root: outputs/diagnostics/psg_scc_curves_impl_20261025/.
Important files: analysis_ja.md, reference_provenance.json, channel_buffers.json,
master_window_audit.json/.csv, reference_pitch_window_check.json,
envelope_coverage.json, inactive_hw_reset_audit.json,
inactive_reset_counterfactual/report.json and corrected_verification.json.

Next implementation is the derived inactive-reset join with native evidence
retained, followed by the SAME six-case loop/buffer/replay checks. Do not invite
user regression as though this change were already active. Re-evaluate envelope
selection from the documented historical policy after isolating this correction.
No private fixture content or generated music is included in the public repo.
This documentation audit made no converter behavior changes and ran no new
fixture conversions. Worktree files are saved; no stage, commit or push.

## Production integration: completed restart record

This section supersedes the earlier diagnostic-only implementation status below.
The user requested saving the unfinished field_notes/handoff first, then a
focused review of x68k long-SCC-note history and further production improvement.
README.md is user-maintained usage guidance: preserve its current contents;
do not add detailed options or technical background without an explicit request.
Detailed switches belong in CLI Usage and focused developer documents.

Saved production code: --software-volume-curves now coalesces derived fixed-tone
PSG boundaries caused solely by inactive hardware-envelope reset BEFORE the
existing single-rise/decay curve stage, reversible structure and shared bank.
The original PerformanceIR, source Segments, controls and boundary evidence are
preserved. Whole ordered source intervals, including zero-duration controls,
are checked for fixed-tone settings, hardware-enable, pitch/mixer operations,
audibility continuity, source gaps and unknown timestamps. Reports distinguish
inactive_reset_joins from curve recognition and preserve original note IDs.

All three CLIs provide mutually exclusive --software-volume-curves and
--no-software-volume-curves. Default remains False; config explicitly forwards
either polarity so future default-on does not lose a caller's legacy choice.
Existing long-curve envelope priority and macro-selection policy are unchanged.
Fallback reports zero actual inactive joins and separate planned joins.

Prior checks completed:52 CLI/config/export/batch checks and17 curve/protection/
playback checks passed. Architect code review found no blocking issue. Split
production conversion/compilation progress is recorded under outputs/diagnostics/
psg_scc_curves_impl_20261025/production_reset_join/summary.json. Full six-case
actual replay equivalence was NOT completed and is not claimed.

The prior final documentation write was rejected because automatic approval
review could not run at the usage limit; code had already been saved. Do not
repeat a diagnostic-only status as current behavior. This checkpoint completes
that missing record. Native reset evidence must never be removed to save bytes.

Next: inspect x68k long-note history before choosing a new curve-selection policy.
The user reports better MML with NEMESIS2 but remaining MGS capacity failures;
record failing song names only, with no new detailed statistics or broad rerun.
Use the six authorized GRA2_001 split VGM/master-MML materials for reference-based
checks. No full-song or unrelated regression expansion. No stage/commit/push.

## Reset-specific actual-driver validation completed

New authored test: tests/script/test_inactive_reset_playback.py. One test covers
four actual MGSC/libkss subcases: tempos75/225 x constant/decaying fixed-tone
volume. A120-frame sounding unit with two inactive evS resets is followed by
rest, restart, an active hardware-envelope note with a real reset atframe156,
another rest and a fixed-volume note. Production reset joins equal2. Both
ordinary and projected paths preserve every effective state over200 frames:
enable, tone period, mixer, amplitude and HW envelope period/shape. The later
active reset remains atframe156 in both. Original controls, boundaries and
Segment reconstruction are retained. The authored check passes.

This turn's focused checks total38 methods (17 curve/protection/playback,
13 tie-compensation,7 loop-first,1 reset-specific actual playback). Prior52
CLI/config/export/batch checks also passed. All six authorized real split
cases compiled, and every source/projected renderer volume hold matched.
Full six-private-case actual playback equivalence remains uncompleted; do not
substitute these authored cases for that wider claim.

The production inactive-reset correction is saved and enabled by
--software-volume-curves. Long-curve allocator changes remain unimplemented;
x68k target ties are not evidence for chopping native sounding units. README
remains unchanged. No stage, commit or push was requested or performed.
