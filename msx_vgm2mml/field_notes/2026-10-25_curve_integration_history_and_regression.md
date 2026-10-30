# Production curve integration: history, restart lessons and user regression

## User-owned documentation

The user maintains README.md for people who want to produce/use MML and MGS.
Preserve its current content unless specifically instructed otherwise. Detailed
switches belong in CLI Usage and focused developer documents, not the README.
The read-only README SHA256 at this checkpoint was
bc2a66d5e00962c490797a802de55a9c84685901946083a1bc86afe8fba6d46d.

## Completed record and production scope

The missing implementation checkpoint is now saved in handoffs/current.md and
2026-10-25_split_curve_analysis_and_inactive_hw_resets.md. Production code
coalesces derived pure inactive-reset PSG boundaries, then runs existing
volume-curve recognition, loop-first planning and shared envelope selection.
Native source evidence is retained. Explicit enable/disable switches are saved
through canonical, export and batch CLI; current default off. A default change
must not erase explicitly requested ordinary behavior.

Retained production results show all six authorized split cases compile with
MGSC. Inactive PSG reset joins for01..06:6,111,78,249,142,95. The stricter
production guards need not reproduce every earlier diagnostic-only join.
Prior checks:52 CLI/config/batch and17 curve/protection/playback checks passed;
architect found no blocker. Full six-case actual-driver replay equality was
not completed. Save measured checks separately from hypotheses and planned work.

## User NEMESIS2 regression, no new conversion

The user reports noticeably improved MML and remaining MGS failures. Only
results.csv was inspected; no reference MML exists for this cohort and no
new detailed statistical analysis or fixture conversion was requested.
Track buffer full / buffer_error prevents MGS for:
GRA2_01, GRA2_03, GRA2_04, GRA2_08, GRA2_09, GRA2_10, GRA2_13, GRA2_16.
Input/output label: psg_scc/vgmrips.net/NEMESIS2_software-volume-curves.
These are an observed capacity indicator, not proof of recovered authored score.

## x68k long-SCC-note history audit

Read x68k notes2026-10-18_scc_two_lane_fm_projection,
2026-10-07_mdx_note_lengths and2026-10-23_msx_structure_parity_and_opm_performed_runs;
checked py/scc_note_units.py, shared mml_envelopes and focused documentation.
The SCC adapter preserves complete inferred notes and nonincreasing same-setting
volume runs. Eight descending runs were explicitly verified as one sounding
unit. Pitch writes, waveform/enable/settings changes and rising volume infer
source-unit boundaries; zero-time pitch controls retain their significance.

The recorded short-chunk rule is TARGET DURATION ENCODING: long MDX durations
are split into legal tied pieces (historically whole-note192-tick spelling;
current performed-run note splitter remains bounded to256 ticks). It is not a
source rule that long SCC notes or envelopes should acquire new attacks.
MSX already splits output notes to255 score steps and software holds to255
frames. Score steps, source samples and envelope frames must not be conflated.

No dedicated prior x68k long-envelope candidate subdivision fix was found in
this focused history/code audit. Do not invent one or promote a remembered
analogy into a rule. Before adding subcurve rebinding, validate tied continuation,
envelope restart, shared-use schedules and channel usage separately. Existing
long-observed-curve priority is an explicit user preference and remains the
baseline for any controlled comparison.

## Lessons to retain

A diagnostic checkpoint must explicitly distinguish planned and enabled behavior.
Document the code and its flags as soon as saved; interrupted approval review
can prevent later documentation even when implementation succeeded. Preserve
English findings outside outputs, because outputs is untracked and privately
backed up. Use the six source-score split fixtures to test structural hypotheses;
do not spend another broad regression budget on unrelated source-only captures.

No source fixtures or README changed. No stage/commit/push.

## Completed source/projected check and historical scope clarification

Retained production results confirm all six authorized split conversions and
actual MGSC compiles succeeded. Source/projected volume-hold CSVs were checked
independently over every renderer frame for both chips in all six cases; they
match exactly. This checks the derived representation, not actual playback.
The local evidence is production_reset_join/projection_hold_audit.json.

This turn re-ran17 curve/protection/playback,13 tie-compensation and7 loop-first
checks:37 tests passed. A new authored inactive-reset actual-driver check is
being added separately; final status must identify its result before claiming
reset-specific playback validation.

The x68k historical rule is target-duration splitting with ties, not invented
source attacks. Existing MGS255-step duration splitting is retained; no arbitrary
long-note/source-envelope chopping or new allocator is introduced. That avoids
silently replacing the user's long-observed-curve selection preference. Future
candidate subdivision needs a separately validated continuation/rebinding policy.

README is unchanged. The user-owned current README's SHA256 was rechecked.
NEMESIS2 source-only user results were read only: GRA2_01/03/04/08/09/10/13/16
fail Track buffer full. No NEMESIS conversion or detailed statistics were run.

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
