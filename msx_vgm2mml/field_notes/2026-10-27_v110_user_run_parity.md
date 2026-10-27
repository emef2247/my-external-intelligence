# v1.1.0 parity against completed user run (2026-10-27)

User completed run_test.sh and requested existing output comparison. No agent
conversion, compilation, listening, or public comparison. Baseline checkout
../../test2/msx_vgm2mml reports git describe v1.1.0. Current branch is
codex/musical_HAL with native integration still uncommitted. User run scripts,
logs, fixture data and generated ordinary output were not modified/staged.

27 local inputs: TWINBEE7, SALAMD14, FMPAC5, contrail1. All staged source
SHA256s match between runs and manifest source/staging hashes. Ordinary MML26
and MGS24 are byte-identical, and their hashes match each publication manifest.
Both runs have24 successes. SALAMD02/06 fail compilation with Track buffer full
in195/214 respectively; their generated MML is still identical. contrail fails
before MML with unsupported OPLL decoder-register range in both runs. Failure
details agree after normalizing checkout-specific report paths. Thresholds
remain FM24ms/normalization8ms. No new ordinary-output regression found.

FMPAC01/03/04 existing keyon comparison missing4/7/4 likewise unchanged; no claim
that equality to v1.1.0 proves original VGM musical/audio fidelity. Current
blueprint success2/blocked24/unavailable1 is outside parity comparison, and
future always-output changes remain pending. This audit doesn't certify all
seven added native-chip routes or complete the outstanding final integration
review by itself. Previous181 integration tests are separate evidence.

Ignored artifacts: outputs/diagnostics/v110_user_run_comparison_20261027/
analysis_ja.md, summary.json, results.csv, current_code_sha256.json, compare.py.
No private fixture payload copied into tracked documentation. README unchanged.

Next user-approved direction after this baseline: always emit blueprint and
musical-layer reference VGM independent of MGS success; strip source volume
from blueprint, embed source timbre parameters/SCC waves and useful score
statistics as comments; indent nested loops with newline at loop start rather
than end, for blueprint and normal MML. This intentionally changes formatting
after the verified parity point. User targets v2.0.0 after completion, then
resume HuC6280/KATOKEN projection errors. No implementation of those changes
was done in this comparison task. Preserve ongoing x68k/MADRV/PDX work separately.
No stage, commit or push performed.
