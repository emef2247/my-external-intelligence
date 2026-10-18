# MSX PSG musical reuse and normalized-loop propagation (2026-10-16)

User requested following the MSX PSG Segment-to-MML implementation after
DSLAYER4 MSX conversions produced MML (three downstream MGSC buffer errors).
That result does not establish MUCOM's separate VGM song-loop seam equivalence.
No private source reconversion, compiler execution or listening performed.

Actual MSX path uses LoopFirstEnvelopeBank -> extract_notes -> source structural
planning -> target rendering. The same extract_notes and SourceLoopPlan already
exist in mucom. Reused extraction rather than copying MGSDRV syntax, envelope
bank limits or its 60 Hz grid. The adapter uses an explicit original-sample-time
view; native PSG State/Segments, real 0xA0 events and clocks stay unchanged.

Software decay/steady levels remain runs in one inferred note; rises, explicit
pitch writes and settings/shape boundaries separate inferred notes. Preserve
zero-duration rows for pending boundaries. New MUC software units initialize E
at entry; HW-envelope units retain existing continuity and ordered G actions.
Source/control comparison keys remain conservative; unique note IDs only control
adjacent grouping. Music CSV maps positive native rows/runs; causal zero-time
writes remain inspectable in ordered native rows, not falsely claimed Key-On.

The previously diagnosed normalization integration hole is fixed independently:
auxiliary source controls contribute every timestamp including redundant,
inactive and EOF writes. Positive intervals between reg13 triggers are protected,
same-clock order is unchanged. The accepted bounded allocation is consumed by
rendering/loop proof; no post-hoc unchecked clock filling or guard bypass.
Normalization diagnostics retain source control identity/register/time and target
clock/error. Unsupported/phase-sensitive loop states can still reject.

Saved code before tests. Ten distinct authored checks passed: eight PSG (three
new) plus two existing normalization feasibility/bound checks. Existing shape
order case additionally passed with normalization and positive-trigger protection.
Authored REUSE artifact under outputs/diagnostics/psg_msx_reuse_20261016 contains
one musical note, volume runs15:1000/10:1000/8:6820 samples. Observed times4410,
4427,4446,8820 have accepted coordinates6,6,6,13; D uses stable_carry, L and ties.
This is intermediate/MUC inspection, not MUB or acoustic validation.

Focused architectural review found no blocker; documented direct music-CSV
membership versus zero-time causal evidence. No commits/staging/pushes or branch
changes. Next: user PSG/MSM regression/listening, inspect newly reported failures
before further edits. OPM/SCC projection and broad regressions remain out of scope.
