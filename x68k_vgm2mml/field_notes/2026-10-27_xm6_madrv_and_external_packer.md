# XM6 guest driver and packer readiness follow-up

User screenshot confirms MADRV2.01a STD/2.06+15/16 started without arguments,
then PCM8A0.60. MCDRV failed because TRAP4 already used. Do not claim the
Rel.X5-S312KiB reference default is the user's actual MADRV allocation.
xm6g.ini: Windrv enabled, Path0=H:/_env/D; ADPCM enabled. Memory=1 is an
emulator enum, not measured resident PCM capacity. No configuration changes.

Native PCM IR already separates encoded codec/data/identity and playback
rate/pan/reset/source time. No source-IR redesign required for mkpdx68.
Need separate target resident budget and PDX-only packer adapter if requested:
existing --pcm-generator also implements MDX compile/inspect API, so cannot
point it directly at mkpdx68. Convert shared immutable bindings to slot=file
list; inspect output bytes independently. Existing write_pdx empty slots
require offset0+size0 (own builder contract); mkpdx uses nonzerooffset+size0,
so adapter/validation must allow that legitimate layout explicitly.
No production implementation/default change in this investigation.

User requested native MXDRV2.06+17Rel.X5-S. Downloaded7144-byte HU executable
with exact embedded version banner from vampirefrog/x68kd11s mxdrv17.x,
blob414f19d4d0e3b0ef1e53a6dd4a5285083f468822,
SHA25656260165a52227aa593de631774ee7e249418fdb6b8c19e4766ec467c940a28e.
Saved H:/_env/D/VGM2MML/tools/mxdrv_206_17_x5s/MXDRV17.X with provenanceJSON;
no guest execution, resident replacement, startup or disk-image modifications.
Ignored detailed Japanese report: outputs/diagnostics/pdx_library_20261027/
xm6_ir_followup_ja.md. MSX run_test remains user-running/pending notification.
