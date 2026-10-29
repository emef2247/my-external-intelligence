# 2026-10-29: saved SALAMD loop comparison and deferred macro default

Latest user steering: choose the macro default after comparing results; do not
immediately switch export_mgs to legacy-macros. Keep current enhanced default
and the three loop modes while the comparison is under review. Existing source
code already includes the prior indentation preservation and self-written SMF1
blueprint MIDI checkpoint; no new MIDI/library/layout algorithm was needed.

Read saved14-song products from test2(v1.1.0) and current SALAMD,
SALAMD_LEGACYLOOPS, SALAMD_SCOREGUIDEDLOOPS. Input-copy SHA256 agrees in all sets.
No source VGM was converted and no existing outputs/logs/test2 files were changed.

                       success  MML bytes(14)  MGS bytes(common10)
v1.1.0                  12/14       182367               76160
current default         10/14       191290               79744
current legacy-loops    10/14       191969               81792
current score-guided    10/14       189715               76800

Baseline failures02/06; all current modes fail02/06/08/11 with track-buffer-full.
Score-guided has the smallest current total MML and common-success MGS, but no
success-count gain. Its MGS common-success total is640bytes above baseline.
SALAMD01 improves11264->9472 MGSbytes with guidance;08/11 remain failures.
MML total includes comments/layout. Never compare MGS totals over different
success sets: baseline also compiled08/11, which current outputs lack.

Important correction to the proposed causal shortcut: v1.1.0's canonical code
also defaulted enhance_macros=True, and saved commands did not disable it.
The enhanced implementation itself and shared compress_macros changed later,
including relative-state preparation. Current legacy-macros skips the later
enhance_macros layer but does not restore all old shared macro preprocessing.
New current exports also request PSG/SCC state approximation at24ms with1/.25
budgets; baseline had no such request. Do not conclude cross-version regression
must be caused solely by the macro switch, even if the loop-mode switch alone
does not recover baseline. A controlled same-code macro A/B is still required
before deciding defaults. No such new private conversion was run in this turn.

Loop modes: default uses LoopFirstEnvelopeBank; legacy selects EnvelopeBank and
old OPLL lowering; score-guided adds native score proposals to the normal path,
retaining exact-command comparison/fallback. Source guide retains attacks and
continuations; it does not erase actual retriggers or read blueprint MML text.

Evidence: ignored outputs/diagnostics/salamd_loops_20261029/{analysis_ja.md,
measurements.csv,comparison.csv,summary.json,compare.py}. Stats only in committed
knowledge; no private music source or MML excerpts copied. README unchanged.

## FM-only state rollback for the next comparison

User requested removing the common state setting as a comparison confounder.
export_mgs again exposes fm-state-ms(default24ms); state-ms is removed.
psg_state_ms, scc_state_ms and both curve error limits are None. Exact software
volume/envelope processing and normalization8ms remain. Macro/loop defaults
were not changed. Batch reruns clear old named curve_approx JSON/decision CSVs
so stale enabled diagnostics do not contradict the new None settings.
The saved SALAMD table above still describes the earlier common-state runs.
User requested minimum validation due quota; no new local_only conversion.
