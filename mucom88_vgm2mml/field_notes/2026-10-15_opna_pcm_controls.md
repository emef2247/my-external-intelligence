# OPNA ADPCM-B live controls and SegaPCM boundary review (2026-10-15)

User authorized PCM continuation support and read-only precision review of
x68k SegaPCM, anticipating future bidirectional PCM conversion. No private
VGM reconversion/compile/replay ran. Existing supplied diagnostics were read
before this implementation; no private music contents are copied here.

Sega source Segments are register observation intervals, not runtime gates.
segapcm_playback maintains chip-specific 16.8 address/phase/delta, natural
stop/loop and native stereo waveform; segapcm_pcm is a separate lossy mono
MSM6258 target adapter. Reuse these boundaries/provenance principles, not that
renderer for Delta-T or the target PcmAnalysis as a universal native container.

Implemented opna_adpcm_playback: immutable whole native epochs and nominal
rational nibble positions from existing AdpcmSegments. Dynamic pan/level/rate
must not create independent decoder starts. Address, memory lowerbits, data,
repeat/prescaler/unresolved memory history remain guarded. Source bytes and
Segments are unchanged; the estimate does not claim fetch/predictor/DAC phase
or waveform equivalence. Source nominal end is recorded separately and does
not cut the new live MUC epoch. Target clock/rate consumption is separately
audited, including explicit normalization omissions.

MUC uses one @sample note followed by identical-code ties. D or changed note
can interact with driver initialization; held v/p setters do not reliably
write live registers. Explicit y0,1,reg,data writes update held level/pan/
Delta-N (including zero-rate pause) without restart. Bank address units differ
from source RAM8, so target pan writes clear memory-mode lowerbits while native
mode evidence remains intact. All positive native intervals, same-time write
order, start count, long-duration chunks and next epoch were tested.

Initial short control hazard: when normalization collapsed the first PCM
slice, the first actual note overwrote prior live y controls. Architect found
this; protect pcm_attack in normalized and baseline clocks. A1sample initial
slice probe passes native control checks after protection.

Windows260407 bundled dotNET is TAG046/2020-04-04 and rejects multiport y.
Official pinned bin.zip f0403762a37028c77afe23193bd674d7bf6efaf7, SHA256
666bc13830a6a9d4748a8f6b9c7c0317f59f2732b7bc12b6c25209600138f2dd,
NET6 profile installed separately under .tools/mucomdotnet-pcm. Official GPLv3
LICENSE.txt retained; no vendor binary/code tracked. New setup script reproduces
installation. Compiler selects this profile only for generated multiport y;
ordinary output keeps legacy compiler. Native replay same profile has measured
startup clock0 versus legacy clock1; verifier records profiles, ±2sample
budget unchanged. Combined public FM + synthetic PCM native FM check passes.

250 related tests: failures0/errors0;1 existing MDX external-helper skip. New
PCM set17 passes without skips, including6 native compiler/replay checks.
WSL public heldctrl.vgm export with implicit8ms and --no-song-loop --timeout600
succeeded. Public baseline validation is recorded under
outputs/diagnostics/opna_pcm_controls/public_validation.json; final tests in
tests_final.json/.log. Public authored fixture is reproducible via
python tests/scripts/opna_pcm_fixture.py --outdir tests/fixtures/public/opna/pcm_controls.

Not implemented: SegaPCM<->OPNA conversion, new ADPCM-B encoding, polyphonic
mix/allocation, repeat/heldPCM source-song-loop certification, initial250..255
levels and unsupported native bus/memory modes. User performs real regression
and listening. No staging, commits or push, unrelated changes preserved.
