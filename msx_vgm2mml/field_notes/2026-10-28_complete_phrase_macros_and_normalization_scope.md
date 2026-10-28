# 2026-10-28: complete-phrase macros and normalization scope audit

User approved loop-first macro selection after inspecting SALAMD02's *7 and *15.
Those were internal volume/tied-hold fragments, not independently complete
phrases. Text savings alone selected them because of frequent occurrences.

## Implementation

mml_macro_phrases groups controls with timed target commands and folds exact
adjacent groups before nonadjacent macro search. Repeated relative volume holds
retain accumulation in finite loops; counts over255 are split. Changing default
length lN is excluded because MGSC compiles a repeat body once: identical literal
text does not by itself prove compiled-duration equivalence across l-state changes.

Candidates prefer multiple attacks, then a complete single-gate gesture with
multiple timed events. Leading tied continuations, endings before another tied
continuation, control-only bodies and isolated single events stay inline or in
finite loops. Nested scope endings without a known release are conservative.
All first-choice alternatives stay within the highest available candidate class.
This is target MML spelling evidence, not recovered native gate identity. There
is no new source note splitting, retiming or software-envelope selection.

The baseline compressor, enhanced allocator and blueprint share the preparation.
Per-node cached shape summaries keep candidate scans from re-expanding subtrees.
Before/after phrase MML and loop JSON make the new pass inspectable; selected
macro CSV has phrase_kind. Prior relative-state normalization remains mandatory.

## Verification

83 focused tests pass, including actual MGSC/libkss PSG playback of descending
held-volume repeats. Register values and order match within4 source-sample
bytecode dispatch jitter. No sample-exact audio claim. Public sample also checks
native Segment equality and ordinary normalization with blueprint independence.

Only the supplied existing SALAMD02 MML was reprocessed into a separate ignored
output directory. Macro definitions32->32; characters23151->22419;428 adjacent
repeat decisions. The two isolated fragment definitions are absent. Effective
timed commands and exit states match. Original and transformed text both still
fail MGSC track-buffer capacity; do not claim this solves buffer allocation.
Original publication and fixtures were not overwritten. No full private VGM
regression. Evidence: outputs/diagnostics/macro_phrases_20261028, including
verification.json, loop audit, selection CSV/JSON, and compiler logs. Profiling
stack dumps in run.log are diagnostic timers, not conversion failures.

## --normalize-lengths scope

The official switch is plural. Ordinary MML applies fitted timing only after a
valid OPLL-derived shared clock is accepted. PSG/SCC-only inputs may abstain.
export_mgs.py already requests normalization via default normalization8ms and
FM-state24ms thresholds; the explicit flag is compatible but usually redundant.

Native-source blueprint retains its independent constrained tempo75 clock and
is not retimed by ordinary MGS normalization. Public sample confirms ordinary
tempo120 normalization while blueprint MML and native Segments remain identical.
Both backends use note-value spelling where representable and percent-step
fallbacks otherwise. No all-note-value guarantee exists. User asked for an audit,
not a new blueprint fitter; that behavior was preserved and documented.

README and user files untouched. Working tree contains this and previous
uncommitted relative-macro work. No commit/push requested. MUCOM remains paused.
