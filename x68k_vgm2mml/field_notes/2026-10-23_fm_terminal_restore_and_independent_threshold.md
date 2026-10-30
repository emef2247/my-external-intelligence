# Terminal-state preservation without whole-gate cancellation

## User direction

After the8ms KNIGHT regression diagnosis, the user accepted improving the
whole-gate cancellation path. Final steering: no public test-pattern sweep is
needed; make1/2/4/8ms selectable and default to1ms. User performs local_only
Sweep Simulation. Do not attribute the regression conclusively to8ms.

An independent --fm-state-ms (default1ms/44samples) now controls eligible
intra-note pitch/uniform-level selection. Existing --normalization-ms keeps
its8ms/352samples default for whole-gate omission and clock boundary movement.
Otherwise lowering the state threshold would unintentionally tighten the
clock, obscuring the intended sweep. No-normalize bypass remains unchanged.
The common selector default is44 in MSX/x68k/MUCOM. MSX opt-in behavior uses
that default; no new MSX CLI option. MUCOM native OPNA/OPM-to-OPNA receives
the independent option in API, clock planning, target filtering and batch CLI;
its separate MUCOMSX route explicitly rejects nondefault values.

## Saved x68k implementation

opm_short_states.prepare returns PreparedStates(positive runs, terminal writes).
A final protected source state with zero target duration no longer cancels
selection for the whole gate. Preserve all original writes rounded to that
release tick in their relative order at the existing renderer terminal point.
Then restore any permitted KC/KF/TL values absent from the current selected
state plus those writes. An omitted prior KF/TL write can be necessary even
when the final original write touches only KC; this is now covered explicitly.

Source-origin terminal writes and inferred target restorations have separate
actions/origins and source-event lineage in the decisions CSV. Actual terminal
writes and full final state remain in performed repeat equality. No source
State/Segments/PCMIR edits and no new positive duration. Original zero-source-
time side effects remain protected; no surviving positive target run still
falls back. Tone/pan/global/key/noise/voice-capacity guards remain.

Standalone .fm.short_states.json now reports independent threshold, omission
count/duration, original terminal writes, restoration count and clock tolerance.
Text export reports distinguish internal-state projection from whole-gate/clock
normalization, including compilation-failed exports.

## Structural / listening rationale

Removing terminal state may reduce commands or allow more loop sharing when
otherwise equal notes have different release states. But key-off leaves a
released envelope, not guaranteed silence. Removing a1ms pre-off frequency/TL
state can affect the much longer release and the state carried until the next
write. Thus the duration threshold is not an acoustic bound. Preserve release
state by default rather than silently introduce a different loss policy. Public
code documents this; listening-based experimental deletion is not implemented.

## Limited verification

No public fixture additions, threshold sweep, catalog reconversion, private
conversion/compilation or listening. Existing unit checks only:9 FM short-state
checks (including compiled temp-authored expectation with missingKF+uniformTL
terminal recovery, zero-target-tail sequence and independent setting),28 batch
export,10 export report,12 renderer,12 performed-run,4 OPNA mapping and7 OPM
normalization checks passed. MUCOM reports6 pureunit checks plus syntax,help,
defaultAPI and diff checks. Test-generated authored data are temporary, not new
public fixture patterns. A temporary report NameError found by tests was fixed
before completion; report suite now passes.

Architect identified the need to restore missing inherited terminal values and
the entire target-zero tail. Both implemented. Final focused review found no
additional material issue. No new KNIGHT success is claimed; user sweep is next.
Code/docs saved in working trees only, not staged, committed or pushed. External
private note repository is separate; unrelated note changes are preserved.
