# Proposed sequencing: finish x68k, prototype control separation in MSX

User agrees complete notes/rests and source repeat structure must precede shared
SW-envelope selection. Proposes MGSDRV as the concrete test target for separating
one attack from pitch/volume/timbre trajectories, since envelopes can be written
explicitly and authored fixtures have known intent. Alternative was a framework
placeholder in x68k first. Recommended: extend existing MSX Note/runs with real
cases rather than place an abstract unused framework in x68k.

Proposed order (not implemented this turn):
1. Close x68k17ms behavior/docs/diagnostics and add DCSG-to-OPM as its own milestone.
2. MSX mechanical catch-up: explicit configuration, batch status/detail/log
   compatibility, and force only where omission semantics are specified.
3. Use MSX existing notes/runs/source-loop/envelope bank as the semantic prototype.
   Test explicit envelopes, held pitch changes versus retriggers, instrument
   changes and repeat entry/exit state. Preserve baseline output as reference.
4. Feed the concrete common contract/adapters back into x68k/MUCOM. PSG drum
   sample-bank extraction is an early application after this source-unit work.

PSG drum bank clarification: current --fm-state-ms affects projected OPM target
states after native PSG interpretation. Continuous PSG-to-PCM directly renders
AY tone/noise/envelope state and is not filtered by that flag. Thus17ms does
not currently improve source drum cutouts or independently reusable PCM banks.
Existing source notes and volume runs can nominate candidate one-shot boundaries,
but shared noise/EG and phase history must remain continuous; a note boundary
is not proof a rendered fragment is an independently playable reusable sample.
Do not cut each Segment or normalize away source envelope/phase to make a bank.

DCSG scope: reuse MUCOM native dcsg.py evidence and relevant musical interpretation;
its dcsg_music.py AY projection is target-specific and should not be routed into
OPM as if native. Current x68k AY projector assumes3parts and one fixed OPM7
noise owner. Explicit OPM allocation/adapters are needed. Dual DCSG tone6 plus
noise2 cannot be declared supported just by8ch count: independent noise sources,
periodic/white noise and tone-linked rates need per-case mapping/approximation/
force-omission decisions. Preserve source and report every target loss once.

Independent plan review agrees with this order and cautions against an abstract
placeholder or indiscriminate17ms across SW-envelope curves. No code edits,
conversion, tests or implementation begun in this direction-setting turn.
