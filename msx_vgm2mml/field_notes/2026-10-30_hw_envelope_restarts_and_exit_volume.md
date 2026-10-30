# PSG hardware envelopes: adjacent notes, not ties; the exit `v15` is observable

Date: 2026-10-30

Status: owner decision plus measured playback evidence on one public-pack
track (Salamander MSX, "03 Fly High", VGMRips). Two converter changes were
prototyped in a scratch worktree and verified by register-level playback
comparison; neither is in the public repository yet. Treat the adjacency
rule below as the owner's stated target policy, not as a chip fact.

## Context

v2.0.0 was expected to reproduce v1.1.0's output. Byte/size comparisons
looked equal, but playing the generated MGS showed two differences on the
PSG drum / hardware-envelope channel: the volume of a long fixed-volume
tone near the song start was wrong, and in drum passages the current code
sounded noisy and rough — "not like drums". Listening found both; the
register traces below explain them.

## What happened

### 1. A numerically redundant `v15` was deleted by relative-state rewriting

The renderers (current and production_v110 `mml_envelopes.py`) correctly
emit `@e8 v15 c%168` for the fixed-volume long tone that follows a
hardware-envelope drum passage: leaving the hardware branch pops the
cached volume precisely so the `v15` is emitted, because a `v` command is
what leaves hardware-envelope mode. The PSG target MML and the
pre-macro intermediates all still contain that `v15`.

It disappears later, in `mml_relative_state.normalize()`, which rewrites
absolute `o`/`v` setters into relative ones using only the numeric
state. The `v15` before the drum passage makes the second `v15` look
redundant (delta 0), so it is dropped. Playback then keeps the channel
in hardware-envelope mode: at 0.42 s the source and v1.1.0 play the
fixed decay 14,13,12,11,10,9 on the volume register with no shape
restart, while v2.0.0/latest wrote envelope-mode volume plus a shape
restart. `signature()` in the same module also modelled only `o`/`v`,
so the macro stage's equality check could not catch the deletion.
v1.1.0 was unaffected because its production macro path does not run
this `normalize()`.

Fix (prototype): `normalize()` tracks hardware-envelope mode per
channel (`s<n>` enters it, any `v` token leaves it, loop exits make it
unknown) and never drops a delta-0 absolute `v` unless the channel is
known to be in fixed-volume mode; `signature()` includes the mode in
the observable state. Two regression tests added. After the fix the
0.42 s decay matches the source and v1.1.0 exactly, and shape restarts
fell 256 -> 214 as a side effect.

Lesson: in MGSDRV MML a `v` command is not a pure volume setter; it has
a mode-changing side effect. Any state-based rewriting of MML must
model the hardware-envelope mode alongside the numeric volume, or it
will eventually delete exactly the token that changes the mode.

### 2. Pitch partitions were tied; the owner wants gapless adjacency

Commit 5ac70e4 ("Preserve PSG hardware envelopes across pitch
continuations") tied hardware-envelope pitch partitions with `&` when
no source shape-reset separated them (245 of 406 hardware-envelope
notes in this track). Playback shape restarts fell to about half of
v1.1.0 in the drum passages (e.g. 8-12 s: 26 vs 65), and the owner
hears the result as smeared and noisy: each hit loses its own attack.
Noise period, mixer, envelope period and tone pitches were identical
between versions; only the restart articulation differed.

The owner's intent, stated repeatedly during development, was the
opposite spelling: **adjacent notes with no gap, not ties**. On MGSDRV
an untied note restarts the hardware envelope anyway, so gapless
adjacency preserves the source timing exactly while every note keeps
its attack. The owner dislikes ties for a second, practical reason:
they split notes into more tokens, lengthening the MML and consuming
the MGSDRV track buffer that already overflows on some tracks
("Track buffer full").

The owner's earlier remark — a hardware envelope is self-running, so
given the start timing, pitch and volume it could be folded into a
loop — was about the *compression* stage. It appears to have leaked
one stage earlier into interpretation (whether partitions are separate
attacks). Compression must not change articulation: loop/macro grouping
may group adjacent `s`-bearing notes, but must not merge them.

Fix (prototype): `psg_hw_envelope_mml.continuation_starts()` now
returns no continuations, so both renderers emit every
hardware-envelope note untied (shape re-emitted per note), and
`docs/project_knowledge.md` states the adjacency rule. The
`.hw_envelope.csv` dump still records every hardware-envelope note.
With both fixes, this track's playback shape-restart counts match
v1.1.0 exactly in total (406) and in every 4-second bucket, and the
target MML opening is again `s9 c+32 < \20 s9 b64 \107 s9 g+64`.

Note the reference subtlety: the source VGM itself restarts the
envelope only 161 times; the game driver could change pitch
mid-envelope. MGSDRV cannot do that with untied notes, and v1.1.0's
per-note restarts are the owner's accepted perceptual reference on
this target. "Matches v1.1.0" and "matches the source restart count"
are different claims; do not conflate them.

## What to do next time

- Judge conversion equality at the playback/register level, not by
  MML or MGS size; both defects here were invisible to size checks.
- When a setter looks redundant, ask what else the command does on the
  target driver before deleting or relativizing it.
- Keep interpretation (attacks, adjacency) and compression (loops,
  macros) separate; a compression idea is never permission to merge
  notes in an earlier stage.
- Hardware-envelope pitch partitions: adjacent untied notes. Do not
  reintroduce `&` continuations for them without new owner direction.

## Follow-ups

- Apply the two prototype patches (relative-state `v15` retention;
  no-continuation adjacency) to the public tree, or reimplement them
  there; the driver-level test in
  `tests/script/test_psg_hw_envelope_playback.py` was updated to
  expect a restart per partition but is skipped where native MGSC is
  unavailable, so run it once where MGSC exists.
- Remaining start-of-song loudness difference vs v1.1.0 (PCM RMS
  500 ms: 1292 vs 1371 after both fixes) is not explained by these two
  defects; the PSG drum channel now matches, so look at other channels
  or macro-level differences if it matters.
- Full test suite in the scratch environment shows identical failures
  before and after both patches (mostly
  `test_conversion_regression` fixture comparisons with a leading
  default-length token difference); the cause of that environment
  difference is still open.
