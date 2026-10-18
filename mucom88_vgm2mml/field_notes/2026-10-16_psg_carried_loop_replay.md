# PSG carried-state source replay for MUC (2026-10-16)

User authorized keeping the initial traversal then reanalyzing the loop body
from the EOF-carried state, with repeated MUC L beginning at replay start.
Motivation: prior user DSLAYER4 regression18/20;05/14 have effective positive
state differences between original loop head and EOF. No private reconversion
or listening performed for implementation.

Use existing shared reader loop_count2 on PSG-containing, enabled-song-loop
MUC paths; nonPSG paths remain unchanged. No interim EOF/reset/Key edge inserted.
Wait overrides and chip/stream state carry through. Main event list, parse_vgm,
MSM source analyzer and Sega trace all use the same count. Assert external PCM
end and projected loop boundary match. Source declared loop/first EOF stay
separate from replay L/expanded EOF in reports. Existing capacity/resource,
source eligibility, phase and seam checks remain; replay is not certification
of envelope/decoder/Sega runtime phase.

SourceOccurrence gives unique analysis IDs while retaining original ID/time,
byte address and loop_iteration. PSG State/control Segment fields and actual
controls expose both. Source state before the next observed write correctly
retains its preceding occurrence as evidence, even across the reader jump.
Short FM omission is occurrence-scoped; original IDs remain audit provenance,
not selectors that remove every iteration of a command.

Saved code before minimal checks:12 distinct authored tests pass (8PSG existing,
3new replay/finite/provenance/wait-carry/mixedMSM tests,1mixedFM omission case).
First gate735samples survives, repeated100sample occurrence alone is omitted.
Authored artifact outputs/diagnostics/psg_carried_loop_20261016: source loop2000,
firstEOF5000, replayL5000, expandedEOF8000. Source head tone288 initially vs352
carried into replay. Native CSV, source/occurrence IDs, stable_carry loop proof,
normalization and MUC L/ties inspected. No compiler or native audio validation.

Architecture review found no blocker. Corrected Sega diagnostic to rendered
loop-body freezing (not 'one recorded traversal' under expanded playback).
Documented scope and resource growth. No stage/commit/push/branch operations.
Next user regression; do not bypass genuinely unsupported phase/decoder state.
