# 2026-10-28: sample-clock score normalization and MGSC notation audit

The user reaffirmed integer44100Hz vgmticks as authoritative; Segment ticks are
legacy compatibility data, not the source of score timing. Review the Oct2
sample-clock adoption and both projects' Oct3 native Segment preparation notes.
An initial score implementation returned to735-sample frames before MML and
misdescribed this as unavoidable backend timing. That adapter has been removed
from score mode. Do not repeat that conflation.

Interpret original sample-clock PSG/SCC partitions and OPLL keyed units, then
truncate complete note durations to multiples of a1/64 at explicit reference
tempo225 by default (configurable). Source State/Segments stay exact. Original
samples remain in the score input; the separately boundary-warped reference VGM
must not become the authority for original durations. MusicalPlan keeps a
score_performance reference separately from the reference-VGM native view.
Device quiet-end anchors share phase among channels of that original chip only.
Quiet evidence is score-gate inactivity, not acoustic release silence.

PSG/SCC interpretation uses clock-tagged sample-valued compatibility copies.
PSG part selection must preserve44100Hz on re-analysis. Software envelope runs
are independently lowered to actual60Hz driver holds; their counters must never
receive sample lengths or score-step lengths. Hardware-envelope registers are
not tempo-scaled. Non225 tempi currently use inline varying volume curves,
because existing tied-SW-envelope compensation is validated at225 only.

Strict duration flooring has no one-sample promotion allowance:734 samples at
225 and1302 at127 stay below a1/64. OPLL's short register slices must be scheduled
inside the complete keyed unit, not independently discarded. Blueprint reports
must describe the clock actually used, record collapsed units, and not retain
stale displacement values from its former minimum-interval clock.

The user corrected the length-inheritance assumption: l32 d1 d d uses1/32 for
the final two notes. Actual native MGSC/libkss probes also show c8c8 retriggers;
spaces do not create a tie. MGSC's documented caret adds length, while & is a
slur. Final notation now selects an explicit default l, omits matching lengths,
and shortens eligible same-pitch full-gate continuations with ^. Guard256-step
combined note limit, physical-line start, intervening controls, unknown macro/
loop octave state and partial q ratios. Bare caret chains and cross-line caret
replacement caused compiler failures in experiments; do not repeat those naive
rewrites. Macro bodies retain explicit lengths to avoid caller default leakage.

The supplied @e list is a software-envelope bank, not the *N phrase-macro bank.
Existing supplied outputs had no exact duplicate macro bodies or envelope
curves. Similar envelopes differ in holds/levels; do not call them redundant or
silently replace them approximately. Longer observed curves and exact-prefix
sharing remain the policy after source loop proposals. Relative octave/volume
factoring before phrase selection remains mandatory. Global macro optimality is
not claimed by the bounded portfolio.

Authored compiler/driver probes and public PSG/SCC/OPLL runs are recorded under
outputs/diagnostics/score_sync_20261028 in the public checkout but ignored by Git.
Text-only recompilation of supplied MML reduced BIKKR04 MGS11776->10496 bytes and
GENPEI01 blueprint3456->2816 bytes; SALAMD02 remains track-buffer-full. Original
products were untouched. These are not full private VGM regressions/listening
claims. MGSC may return0 while producing an empty MGS on error: inspect size/log.

Stable public rules are in docs/score_normalization.md and project_knowledge.md.
README remains user-maintained. No task commit or push has been requested; save
this note separately from public code if a later commit is requested. Preserve
all unrelated untracked notes in this private repository.

Review regressions added: original source durations must feed score, not the
warped reference. A rest crossing a device quiet anchor uses mapped endpoints;
flooring its independent duration can overrun the next synchronized onset.
Skip collapsed partitions before emitting a leading rest. Clip blueprint pitch
changes at/beyond the truncated score end and recompute its timing diagnostics.
Preserve score_performance's original hash in publication, not the reference
view's hash. OPLL trims a complete keyed group, not every short register slice.

User also reiterated that vdiff/odiff/ldiff describe differences from the
immediately preceding observed row. Retain this motivation for repeated relative
volume/octave/length-gradient gestures, rather than treating them as differences
between only the selected macro events. Python's current named fields are not
identical to all old Tcl fields; do not invent an ldiff member or reinterpret
legacy differences while changing the score clock.
