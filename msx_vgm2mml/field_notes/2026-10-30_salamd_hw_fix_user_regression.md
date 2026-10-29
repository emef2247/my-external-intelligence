# User SALAMD regression after HW-envelope fix — 2026-10-30

User requested record-first investigation because only1%quota remained.
Read saved products/reports/logs only. No conversion, compilation, driver/audio
replay or converter changes. The two reported commands were export_mgs SALAMD
--force, and SALAMD_SCOREGUIDED with --score-guided --force. The abbreviated
switch was accepted: all14guided report options explicitly show
score_guided_loops=true,loop_mode=score-guided,macro_mode=v110; all14normal
reports showfalse/v110/v110. Both use FM24ms, normalization8ms, software volume
curves on, no PSG/SCC state approximation, ordinary score-normalize off,
blueprint-score-normalize on. Therefore this is not the earlier enhanced/hybrid
six-way dataset. Do not confuse SALAMD_SCOREGUIDED with older
SALAMD_SCOREGUIDEDLOOPS or LEGOCYMACROSCOREGUIDEDLOOPS.

Both conditions succeed12/14: SALAMD02 and06 still fail with track-buffer-full.
Normal failure lines174/190, guided178/191; line numbers are source locations,
not buffer byte quantities. All other songs including03/08/11 compile. Prior
v1.1.0 also succeeded12/14 with02/06failed. Compilation success is not listening
validation or proof that the reported PSG drum sound is corrected.

| Song | Normal MML bytes | Normal MGS bytes | Guided MML bytes | Guided MGS bytes |
|---|---:|---:|---:|---:|
| SALAMD01 | 12275 | 11136 | 11627 | 8960 |
| SALAMD02 | 22952 | failed | 23145 | failed |
| SALAMD03 | 14779 | 10112 | 14779 | 10112 |
| SALAMD04 | 8636 | 5120 | 8721 | 5120 |
| SALAMD05 | 15093 | 11008 | 15068 | 10880 |
| SALAMD06 | 23496 | failed | 21390 | failed |
| SALAMD07 | 13165 | 11136 | 13165 | 11136 |
| SALAMD08 | 13021 | 10368 | 13021 | 10368 |
| SALAMD09 | 6462 | 4480 | 6472 | 4480 |
| SALAMD10 | 5785 | 3328 | 5785 | 3328 |
| SALAMD11 | 16714 | 14336 | 16714 | 14336 |
| SALAMD12 | 10807 | 8320 | 10788 | 8320 |
| SALAMD13 | 14119 | 9216 | 13931 | 8960 |
| SALAMD14 | 3761 | 1536 | 3761 | 1536 |

Same12successful-song MGS totals: normal=100096B; guided=97536B; guided-normal=-2560B.
Saved pre-HW-fix v1.1.0 total for the same12songs=100992B. Differences from that baseline mix the intentional HW-envelope correctness fix with loop policy; do not attribute every change to score guidance.
Guided smaller: SALAMD01, SALAMD05, SALAMD13. Guided larger: .

Failed outputs are excluded from MGS totals even if an older publication remains
on disk. MML file size includes comments/layout; these are file sizes, not
per-channel compiler buffer usage. Source/control/reference fidelity and actual
drum listening remain pending user feedback. No new default choice is made.

References: 2026-10-30_psg_hardware_envelope_drum_retrigger.md (fix/probe),
2026-10-30_production_v110_default_and_preserved_profiles.md (profile history),
outputs/psg_scc/vgmrips.net/{SALAMD,SALAMD_SCOREGUIDED}/results.csv,
_diagnostics/<stem>/report.json and logs/local_only/SALAMD*.log.
