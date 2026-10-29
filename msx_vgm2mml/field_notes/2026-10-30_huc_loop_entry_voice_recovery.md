# HuC6280 / SCC voice recovery (2026-10-30)

The saved BIKKR regression contains 24 inputs. Ten used whole-source-channel
force subsets and one was blocked. Some inputs genuinely contain fewer than
four active voices. In the inspected loop losses, short first/second-traversal
register differences caused whole channels to be excluded, rather than an
inability to represent their ordinary waveform mode in SCC.

Add a bounded force-only target view: restore the observed first-traversal
register state at each loop entry. The source trace, native Segments and native
reference VGM remain unchanged. This deliberately repeats the first traversal's
entry behavior instead of claiming the input's later traversal is identical.
The inserted controls have null original event IDs; retained commands keep
original source event IDs. The projection report identifies the approximation.
Both normal native-view and independent target-loop validators still run.

The route rejects active DDA/noise/LFO, source issues and any register6 writes
inside the loop; register restoration cannot establish RAM cursor/phase safety.
Existing strict behavior and fallback subset behavior remain available. No new
OPLL/PSG supplementation was introduced; pre-existing compatible routes remain.

Focused results (projection, not listening): BIKKR04 SCC2 -> SCC4;
BIKKR09 SCC1 -> SCC3 (source3); BIKKR14 SCC1 -> SCC5 + existing PSG1;
BIKKR24 blocked -> SCC3 (source3). BIKKR04 full export with
--score-guided-loops --force generated MML and MGS successfully; control CSV
contains four audible SCC destinations. Forty-six HuC authored/integration
checks pass. User owns subsequent listening/regression.
