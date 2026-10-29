# SALAMD saved comparison after FM-only option rollback — 2026-10-30

User supplied six commands: enhanced/default macros x normal/legacy/score-guided
loops, and --legacy-macros x the same three. Read saved artifacts only. No private
VGM conversion, compilation, playback, source edits, default changes or commits.

Available current results are SALAMD, SALAMD_LEGACYLOOPS and
SALAMD_SCOREGUIDEDLOOPS. Each contains14results. The three additional legacy-macro
folders are absent from test outputs and test2 outputs. Their same-named logs
exist under test2/logs/local_only but are0bytes. Asked user asynchronously about
location; do not silently run missing private regressions or mix stale results.

Baseline: ../../test2/msx_vgm2mml/outputs/local_only/psg_scc/vgmrips.net/SALAMD.
All staged VGM hashes match baseline for the three available modes. Current
PSG/SCC thresholds/error budgets areNone; approximation report is{}. Existing
normalization8ms/FM24ms/software curves are shared; score-normalize isoff.

                       success  MML total14  MGS common10
v1.1.0                  12/14     182367          76160
current normal          10/14     191799          79744
current legacy-loops    10/14     192514          81792
current score-guided    10/14     190224          76800

Baseline failures02/06; current allthree additionallyfail08/11. Channels:02ch1,
06ch2,08ch5,11ch5. Normal and score-guided allocation matches baseline exactly
for08(ch5=3685) and11(ch5=6489); this is not explained by decreased #alloc shares.
Do not interpret physical compiler line numbers as comparable note positions.
#alloc is a specified capacity, not a measurement of compiled track bytes.

The earlier shared-state24ms artifacts had different MML text totals, but the
common-success MGS totals and failed-song sets were the same. Removing PSG/SCC
approximation alone did not resolve the regression. Allcurrent modes use enhanced
macros, and v1.1.0 also defaulted to enhanced, though the implementation/shared
preprocessing changed. Missing same-code legacy-macros A/B remains necessary
before choosing defaults or attributing regression solely to macro flags.

Macro definition totals baseline410 vsnormal393,legacy-loops395,guided392. Fewer
macro definitions do not guarantee smaller compiled channel buffers. Guided is
best of current loop choices by aggregate size, still640MGSbytes above baseline
on common10successes, without recovering08/11. It is not uniformly better per-song.

Ignored evidence: outputs/diagnostics/salamd_fm_only_compare_20261030/
analysis_ja.md,measurements.csv,comparison.csv,summary.json,effective_options.json,
channel_allocations.csv,failure_channels.json,compare_saved.py. Reports contain
stats/paths only; no private music text was copied into notes. Existing outputs,
logs and test2 were unchanged. README untouched. Keep macro/loop defaults pending
complete six-way saved results. Next action: locate user results, then extend
comparison; do not launch another private conversion without authorization.


Follow-up during this turn: new legacy-macro products began appearing in the
correct test checkout while comparison was underway. They are still partial;
do not treat the earlier missing-output snapshot as a completed failed run.
Await all14songs in each of the three added modes before the six-way decision.
