# Remaining PSG song-loop state mismatches (2026-10-16)

User regression after MSX note extraction/clock propagation:18/20 exported;
DSLY4_05 and DSLY4_14 still stop at musical_structure_and_rendering. User asked
for details. Read existing diagnostics/JSON/native Segment/music/timing CSVs
only. No conversion, test, compiler run, source edit or listening performed.

Diagnostics:
-05: outputs/psg/vgmrips.net/DSLAYER4/_diagnostics/DSLY4_05/run_f2j9nf1g
-14: outputs/psg/vgmrips.net/DSLAYER4/_diagnostics/DSLY4_14/run_qj4cw5cj

Both normalization maps now contain every observed loop-control timestamp,
including EOF; missing count0. Bound respected (05 max349.7693;14 max351 source
samples). No FM/PCM gates omitted and0force omissions. The former missing-clock
integration defect is resolved in these outputs.

New rejection is prove_loop_states: SSG inherited loop state differs during
positive target time; observed head restoration is delayed or incomplete.
Both E and F reject in each manifest. Renderer reports only the first crossing
unit it cannot render: E for05, F for14. No accepted SSG seams recorded.

05: source loop282232 (~6.39982s), EOF1411196; target loop clock2217,
Timer-B246 (127.211538 source samples/clock). First compared interval2217..2218:
E tone period267 first traversal vs318 from EOF, software volume9;
F tone period807 vs1077, software volume12. Loop-head event1015 only writes A
pitch. E changes volume at+3681samples and becomes silent at+8826samples before
its pitch pair is updated at+11769/+11771; inherited E pitch therefore matters
for audible positive time, not just a discarded same-tick initialization.
F pitch writes begin at+50/+52samples. All observed coordinates exist.

14: source loop376334 (~8.53365s), EOF3857297; target loop clock3286,
Timer-B247 (114.490385 source samples/clock). First compared interval3286..3287:
E noise-only(period10, volume5) vs tone-only(period702, volume8);
F tone period1281 vs1209, software volume9. Initial loop actions update A level
and E pitch low byte; E high byte follows at+49, mixer+54, E level+67samples.
F pitch pair follows+110/+112samples. The accepted ordered map separates some
of these short positive intervals; no missing-timestamp issue remains.

No HW envelope is active in these mismatching states. This is neither a missing
noise/EG feature nor a compiler/bank limit. It is the conservative single-pass
song-loop proof demanding equivalence between original head seed and EOF seed
through positive target time. Raw VGM looping naturally carries EOF state;
initial and repeated traversals need not sound identical, and that alone is not
source corruption. Current self-contained first-traversal MUC/L representation
has no certified handling for that difference. Shared finite phrase structure
and native VGM whole-song loop semantics are distinct.

Potential follow-up (not implemented/validated): retain first traversal as
intro/transitional material, replay loop commands from EOF-carried source state
into native evidence, then choose a proved repeated-traversal L boundary. Do
not merely remove the guard or force head reset (would change repeated audio).
Need source/target/timing/structure audit and native source identity mapping;
HW envelope/noise phase and eventual stability need separate consideration.
Existing --no-song-loop avoids this boundary requirement as a finite-output
workaround. Current --force does not register/omit loop-state mismatches, hence
0omissions is consistent with failure. User owns private regression/listening.
