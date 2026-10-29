# PSG hardware-envelope drum retriggers and MGSC tie order — 2026-10-30

User heard incorrect PSG drums in SALAMD03 from
SALAMD_LEGOCYMACROSCOREGUIDEDLOOPS. Attached MML was a PSG-only audition view;
its32macro bodies match the saved hybrid legacy/guided result, rather than
identifying it as a fresh v1.1.0-default export. No source payload is copied here.

Source/probe evidence in ignored outputs/diagnostics/salamd03_psg_drum_20261030/:
compile attached MML and export5seconds via native MGSC/libkss. Source begins
with one shape9 write at26VGMticks; generated playback repeats shape9 at775,
2232,2965,18420ticks. Startup delay is separate from extra reset count. Hardware
envelope period416 is correctly encoded, but pitch partitions retrigger it.

Two authored driver probes isolate the issue:
s9 c64 d64 e64 writes shape9 three times; s9 c64 & d64 & e64 writes once.
Critically, c64 backslash-detune & d64 still retriggers, while
c64 & backslash-detune d64 keeps the envelope running. MGSC attaches & to the
preceding note, so intervening controls before & break the intended tie.
Do not confuse this with note whitespace/default-length semantics.

Fix: derive contiguous HW-envelope continuations from accepted PerformanceIR
partitions, preserving explicit hardware_envelope_reset boundary reasons. Emit
& BEFORE following mode/noise/octave/detune/period controls; omit new s/v on
those continuations. Genuine source resets, rests and HW/non-HW transitions
retain new-attack behavior. Native State/Segments/partitions remain unchanged.
A .hw_envelope.csv decision dump retains channel,timing,shape,continuation and
source Segment indices. Both current and isolated production_v110 renderers
receive this correctness fix: strict v1.1.0 binary identity is intentionally
not promised for inputs affected by the inherited HW-envelope bug.

Two authored tests pass. Actual MGSC/libkss playback across6pitch partitions
keeps2real same-shape resets in current LoopFirstEnvelopeBank,
ScoreGuidedEnvelopeBank and historical production_v110 LoopFirstEnvelopeBank.
This is bounded behavior verification, not full SALAMD/native audio equality.
User explicitly requested prompt code checkpoint due3%quota and owns full
SALAMD regeneration/listening. No full local conversion or broad suite run.
No claim of complete multi-channel shared-HW-envelope phase reconstruction.


## User regression in progress

User is regenerating SALAMD after the fix, initially without --score-guided-loops,
and plans an additional run with score guidance enabled. No result/listening
confirmation has been reported yet. The HW-envelope continuation fix is in both
normal and score-guided production renderers; the switch is not required to
activate it. Do not start a duplicate local regression or claim either user run
has passed. Implementation public commit5ac70e4; initial private record4c6be77.
