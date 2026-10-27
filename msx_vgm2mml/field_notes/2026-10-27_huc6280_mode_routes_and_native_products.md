# HuC6280 mode-aware projection and independent native products (2026-10-27)

User requested checkpointing score/blueprint changes on a separate branch before
continuing HuC support while they audition the score/curve work. Public branch
codex/score-guidance-huc6280 started at2a2f3b4; prior private notes46fa6f1. README,
user run scripts/logs, private inputs and ignored generated artifacts untouched.
No push authorized/performed.

The source NativePerformance/ScoreGuide now precede HuC target preparation.
Typed B9 controls retain all bytes, including reserved bits, selected-channel
and DDA/waveform register6 context. DDA commands are not duplicated as PCM
packets; DDA units are PCM and excluded from blueprint. Source controls and
units preserve volume/balance/pitch/wave/enable evidence. Active LFO phase/pitch
is not guessed. Both successful and failed MGS lowering must select original
HuC music for the reference writer. Building source first was insufficient:
a successful target branch still selected the accepted MSX execution until a
new success-path native-command comparison caught it. Test dispatch, not just
source-layer existence or a report status.

Keep initial static SCC/PSG behavior. Extended stable(channel,mode) routes support
disabled RAM wave changes, AY noise, and OPLL ROM overflow. Five SCC voices require
compatible waves through all positive overlaps of the shared pair. Enabled muted
oscillators still advance: their RAM writes are not safe stopped uploads. AY
noise is one shared generator. Different projected rates at a positive wait fail
explicitly; zero-time intermediate changes can settle. Reconcile shared-resource
demand at every wait: per-owner cache keys alone can leave a transient owner's
wave/noise rate on a continuing voice. Added authored regressions for both.

Pinned c6280_mame noise rate derivation: step=(clock/rate)*32/(32-low5), threshold
0x800, hence clock/[64*(32-low5)]. An initial research message used32 instead64;
local primary code resolved this. AY shift clock=clock/(16*period), periods1..31;
clipping and cents error retained. Sequences/spectra differ.

OPLL ROM1..15 numeric harmonic/RMS catalog is reproducible via optional pinned
libvgm cores (bad50c571db45b872304bdcd0603dd2214f0663e), no private wave data or ROM
bytes stored. Fit16 harmonics at three pitches; exclude strongly decaying held
presets. No global custom patch sharing problem. Report preset/error/DC loss;
reference gain is not hardware calibration. Preserve last frequency throughout
key-off/release, including subsequent inactive updates. Key-off is not immediate
silence. MGS sustain loss diagnostics distinguish target execution from native
source/reference. DDA/LFO and mixed-chip MGS lowering remain unsupported; native
products survive. Extensionless Vgm-signature inputs discovered without rename.

Architect review found shared-resource transient-owner corruption and OPLL
key-off pitch clearing; both fixed with authored tests. Review quota ended
before another round. Related285 tests passed; final25 focused checks including
canonical output and actual MGSC/libkss SCC+OPLL+noise playback also passed.
Two older adjacent-layout assertions were updated to require the already-default
blueprint directory, rather than removing the directory to satisfy stale tests.

Scoped local validation only: BIKKR06 success(MML5105/MGS2432 bytes), DSPILT01
success(1743/512), KATOKEN11 success(2546/1152). Real MGS->libkss VGM has nonzero
assigned-chip output: BIKKR PSG/SCC; DSPILT SCC/OPLL; KATOKEN SCC/PSG noise.
DSAVER13 explicitly fails MGS for DDA but native VGM and non-PCM blueprint succeed.
All4 input hashes unchanged, ordered original B9 commands and source durations
match reconstructed native VGM exactly. Original/output command counts:
2621,773,1859,26290. This is not acoustic or score-source equivalence proof.
Source guide mappings inspected: BIKKR PSG128/SCC224, KATOKEN PSG54/SCC65,
DSPILT SCC18 accepted notes have source guide IDs. No broad corpus run/listening.

Evidence (ignored): msx_vgm2mml/outputs/diagnostics/huc6280_remaining_20261027/
summary.json, analysis_ja.md, unit_tests.log, exports with source/target pass CSVs,
MML/MGS/native reference and MGS playback VGMs. User handles broad regression.
Next: user listening/bug reports; do not silently invent DDA/LFO MML or assert
OPLL/noise equivalence. Preserve user README/run scripts/logs and input files.
