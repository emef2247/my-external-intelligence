# Native MUCOMSX musical-route checkpoint

2026-10-19. User requested saving a checkpoint and updating README so private
regression can proceed. No public/private Git mutation was requested or made.
The older SCC VGM enhancer remains suspended.

Read inherited MSX field notes before expanding the new target: complete
source musical units precede envelope assignment, reversible outer/inner source
plans precede target macros, zero-time key/control evidence must not disappear,
and source timing remains separate from target clocks. Macro source savings
must not be advertised as equivalent compiled-byte savings.

The source adapter retained all raw commands and native Segments, added explicit
EOF analysis views, and reused initialization classification for inactive SCC
resets. Sixty existing public MSX fixtures passed source analysis. Ordinary
public input sets were added to a repeatable verification harness.

The renderer uses native PSG, three native SCC parts with OPNA additive
projection for excess SCC, native OPLL and optional OPNA rhythm layering.
DCSG single/dual source evidence and derived AY views were also saved, as was
explicit detuned PSG/SSG layering. Separate source-only reference analysis is
in 2026-10-19_dcsg_dual_source_and_detune.md.

Important correction at the user's checkpoint request: OPLL source channels
7–9 must not silently become OPNA voices. That automatic overflow policy was
removed. The pinned official MUCOMSX native MUC interface exposes six melodic
MSX-MUSIC parts plus rhythm, and converts OPNA-style DAT parameters to ROM
presets. This is not a YM2413 hardware six-channel limit or direct arbitrary
YM2413 patch pass-through. Future OPLL-to-OPNA enhancement/preset reference
work was explicitly deferred; no Nuked-OPLL source/data was imported for it.

Authored native PSG/SCC/OPLL MUCs compiled/exported with the installed tools.
Exact SCC waveform bytes and the target OPLL octave correction were inspected.
Native software-volume commands must follow a tie marker, not precede it:
the compiler's same-note optimization otherwise removed held volume writes.
Native software volume can take one driver tick to appear; the current timer
budget reserves half the requested boundary bound for this measured latency.

Authored native SCC gain probe gave v0..15 output levels:
0,0,0,0,0,0,1,1,1,2,3,4,5,8,11,15. The inverse is target-specific and quantized,
not an identity mapping of linear SCC volume. Native raw-volume y writes were
not adopted: a manual exploratory probe left an unfinished VGM and is not a
passing validation artifact.

The runtime adapter validates finalized VGM framing/timing and stable output
hashes, not just a DOS completion marker. DOS/ROM images and local profiles stay
ignored; original images are copied, never modified.

Earlier full MUC inventory predates final OPLL policy/timing changes. It found
source-size failures in long mixed compositions; recursive exact MUC macro
search recovered several. Rejected text, source/target dumps and per-phase
reports remain under outputs/mucomsx_public_20261019. These are not final-current
whole-suite success results. Full native round-trip agreement is unfinished.

Next: user private regression. Then verify native OPLL interface/custom-patch
constraints before designing requested OPNA melody layering; complete public
native comparison. Single-DCSG independent noise on a free SSG slot remains a
checkpoint item; current dual-chip noise uses explicit shared-volume losses.

Final checkpoint: 87 focused helper/source/conversion/CLI/DCSG/runtime/reporter
checks passed after the OPLL policy correction. No full new native catalog run.
