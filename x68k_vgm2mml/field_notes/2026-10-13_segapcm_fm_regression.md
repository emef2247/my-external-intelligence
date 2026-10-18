# SegaPCM FM-only user regression and MML duration limit (2026-10-13)

User explicitly permits identifying local_only fixture filenames in these
external private notes. Public project documents remain anonymous.
This investigation only read existing private logs/CSV/MML/MDX. No private
reconversion, compilation or playback ran. SegaPCM->PDX remains deferred.

TBLADE: TBL02 MXC lines42/271 contain r1..=336 and r1.=288 ticks.
TBOUR: TBOUR01 lines705/748 contain r1.=288 ticks. These exceed MXC's
single-duration256-tick maximum; the issue is not excessively fine resolution.
Canonical timed rendering now splits long rests and retains tied-note splitting;
rest compaction cannot reintroduce an oversized remainder. MXC syntax adaptation
also splits long dotted rests from older generated scores. Native source timing
is intact.38 focused tests pass. Two authored scores compiled with native MXC,
and independent encoded-command census gave960 ticks each.

TBL01 and TBOUR04: run68 invalid-address write at PC0352C4, debugger prompt,
then wall-clock180-second compilation timeout. Increasing timeout does not fix
the invalid write. TBL04/TBL11/TBOUR07 exit-11. TBOUR06/TBOUR08 report an
unclosed loop despite balanced source brackets; their clock counters include
abnormal values (1308504066 for TBOUR06 A,33437495 for TBOUR08 H).
Prior field_notes/2026-10-11_mxc_size_failure.md has authored reproduction of
size-dependent native compiler false-loop errors/crashes. This is a plausible
related capacity failure, not proof of the same internal cause or a universal
hard MDX size threshold. Try --compiler mmlx in a separate output directory;
MDX offset/player buffer limits remain independent.

TBOUR05: conversion_timeout after native evidence exists,142048 Segments.
No final MML. Target clock/normalization/music/structure is downstream, but the
exact expensive function is unprofiled. --timeout600 is a user trial. Optional
--no-song-loop reduces replayed source volume but makes finite one-pass output.
180 seconds is a per-stage wall-clock budget, not a song duration or chip clock.

TBOUR02 MDX36187 bytes, largest track F8859 bytes; user reports MMDSP track
buffer shortage. Configured player limit unknown. TBL10 controls/counter work,
level meter does not animate. Selected clock9984us (@t217), not256us. Tracks
A-D/H have no ordinary NOTE, and use raw OPM control; display-cache bypass is
an unconfirmed hypothesis. --normalization-ms16 can reduce timing/control
precision and omit whole gates<=16ms, but does not guarantee display recovery.

Detailed Japanese report is ignored public-project output:
outputs/opm_segapcm/fm_regression_summary_2026-10-13.txt.
Stop for user regression. No commit/stage/push/branch change.
## User mmlx retry (2026-10-13)

Existing updated results.csv/error logs were inspected read-only.
TBLADE:12/12 success with mmlx. TBOUR:5/8 success.
TBOUR04: data inconsistency: MDX track7 offset exceeds maximum0xfffe.
This is a target track-start offset limit, not a timeout or256-tick duration.
TBOUR06: loop start value -50674 cannot be represented in MDX.
The compiler reports a loop-position representation limit. The exact offending
loop has not been identified; do not infer malformed MML or a particular track.
Neither failure is repaired merely by increasing wall-clock timeout.
TBOUR05 remains conversion_timeout after180seconds; the user will retry with a
larger budget. No timeout default was changed. No private build was run here.

README now has TROUBLE SHOOTING recommending --compiler mmlx for
compilation_failed, preserving required input options and warning about remaining
MDX/player capacity constraints. Removed the previous introductory timeout
paragraph per user direction; no timeout advice was added to this section.
Documentation-only update; no additional tests or Git mutations.
## TBOUR06 loop limit clarified

Read installed mmlx0.2.0 src/mdx/compile.rs: MmlCommand::LoopStart records the
song-loop marker; track finalization computes marker_position-track_length-
adjustment and requires i16 (lines223-270,589-590). The reported "loop start"
-50674 error is the song end jump back to L, not a finite [ ]n repeat and not
the original VGM byte offset. Native MML includes L on each active track.
--no-song-loop removes declared source-loop replay/song-loop output, preserving
finite pattern compression. This is a valid finite-output trial for TBOUR06;
success is not guaranteed if track-offset/player limits are subsequently hit.
No private conversion or new implementation ran. Optimizing compiler remains
future work per user; user will retry with --timeout600 separately.
## User timeout600 result

Latest TBOUR results.csv/error logs confirm five exports still succeed.
TBOUR05 now completes conversion and reaches mmlx: "loop start value -47768
cannot be represented in MDX". The previous180-second conversion timeout was
not a source rejection; the600-second budget permits reaching compilation.
TBOUR04 still exceeds track7 offset0xfffe. TBOUR06 still reports loop start
-50674. Thus TBOUR05 and06 can be tried with --no-song-loop; TBOUR04's
track-offset issue is separate, although finite output may reduce total size.
Do not claim finite-output success before user execution. No private rerun
performed by the agent; user owns listening and conversion regression.
## User finite-output regression

TBOUR_NO_LOOP/results.csv confirms7/8 exported with mmlx --no-song-loop,
without an explicit timeout override. TBOUR04 and06 now succeed (generation
only; playback not newly reported). TBOUR05 completes conversion but fails:
"data inconsistency: MDX track4 offset exceeds the maximum0xfffe".
This is a new surfaced size diagnostic after removing the song-loop jump;
it is not a conversion timeout. Raising timeout cannot itself repair this
track-start representation limit. Do not promise the planned600-second retry
will solve it. README TROUBLE SHOOTING adds finite-output trial and retains
finite-repeat compression. Default180 remains unchanged pending user's tests.
Agent inspected existing results only, no private build or playback.
## TBOUR05 finite timeout600 capacity diagnosis

Latest TBOUR_NO_LOOP_TIMEOUT600 results/log retains "MDX track4 offset exceeds
the maximum0xfffe". The conversion has finished; timeout default remains180
by explicit user decision. This is not a PCM target failure or duration limit.

soundlog0.15.0 src/mdx/document.rs lines86-119 calculate relative positions
from header.base_offset and iterate tracks with zero-based enumerate. Track4
is E. The A-D data already precedes E; the start offset must be<=65534, and
65535 is excluded as the absent-track sentinel.
Read-only generated-MML census (counts are encoded-body tokens, repeats not
expanded): raw y counts A2657/B2544/C11509/D11877, total28587. mmlx0.2.0
src/mdx/compile.rs lines600-606 emits one MdxOpmRegisterWrite per y. soundlog
command.rs assigns three bytes to that command. Thus raw register commands
alone preceding E account for85761 bytes, even before rests/notes/repeat
commands/header/tone data. The cumulative layout is too large; E itself need
not be an oversized track. MML text length669762 is not the MDX byte length.

This directly grounds the volume diagnosis without compiling private input
again. Altering timeout, stripping text whitespace or merely increasing timer
period does not resolve the accumulated register bytes. More output-level
compression/optimization or a different target would be needed; optimization
compiler integration remains deferred by the user. No implementation change,
private build/playback or Git mutation performed in this follow-up.