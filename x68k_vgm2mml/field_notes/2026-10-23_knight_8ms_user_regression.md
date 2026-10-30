# KNIGHT user regression after the 8 ms FM selector

User reports12/18success, with14newly failing versus the prior13/18run.
Read existing results.csv, compiler error logs, normalization JSON, short-state
CSV/JSON, report text and generated MML only. No local_only conversion,
compilation, playback or source reanalysis performed; no production code change.

## Confirmed failures

All six failures are mmlx MDX track-start offsets above0xfffe, not PDX capacity,
invalid MML syntax or converter failure.14uses native OPM only, with no PCM.
Current normalized clock is15872us/tick, maximum boundary movement350samples,
8ms threshold352, whole-gate omissions0. Song-loop emission is disabled in the
saved JSON, so the pasted command's line break is not evidence that flags were
omitted from the actual run. Finite repeat compression remains enabled.

- KNIGHT03: data inconsistency: MDX track 4 offset exceeds the maximum 0xfffe; omitted states1833; fallback states3296; fallback gates1289.
- KNIGHT10: data inconsistency: MDX track 3 offset exceeds the maximum 0xfffe; omitted states2002; fallback states11656; fallback gates5784.
- KNIGHT11: data inconsistency: MDX track 6 offset exceeds the maximum 0xfffe; omitted states1087; fallback states4335; fallback gates1674.
- KNIGHT13: data inconsistency: MDX track 8 offset exceeds the maximum 0xfffe; omitted states1225; fallback states537; fallback gates131.
- KNIGHT14: data inconsistency: MDX track 8 offset exceeds the maximum 0xfffe; omitted states673; fallback states5358; fallback gates2324.
- KNIGHT16: data inconsistency: MDX track 4 offset exceeds the maximum 0xfffe; omitted states1488; fallback states306; fallback gates47.

## KNIGHT14 structure observations

Current MML151639bytes is text, not the unavailable compiled MDX byte count.
Finite repeat openings843 overall; E has59. E is the largest written track at
54697characters; raw y writes3548, D setters1589. These are static written-syntax
counts, not expanded execution or exact compiled-byte attribution.
14has673applied short-state omissions;5358short rows are not_applied because
the protected positive release-tail interval collapses on the target clock.
E alone has316omitted intervals in160gates versus1993fallbackgates.

The shared-duration-pattern audit found16short-duration shapes with both
applied and fallback outcomes. For E, run0=4samples and run7=1sample occurred
with30applied/56fallback; run0=4samples alone with10/64. This CSV omits long
intervals and full states, so these are NOT proof of identical musical phrases.
They corroborate inconsistent adoption across timing phases, not equivalence.

## Interpretation and next candidate

The new selector drops states, but dropping states does not guarantee a smaller
compiled program: complete-gate fallback can mix filtered and unfiltered
representations and thereby prevent exact repeat folding. Current code's
release_tail_collapses_on_target_clock guard returns None for the whole gate;
caller uses the original path. Loss of repeat sharing is a plausible cause of
14regressing. The prior successful14MML/MDX is no longer present, so the exact
before/after byte delta and causal attribution are unconfirmed. Do not claim
raising12/16ms alone fixes this; the release guard is independent of threshold.

Preferred next implementation candidate: retain the terminal register state
explicitly with validated key-off/release ordering, while allowing safe earlier
pitch/level state selection, instead of abandoning the whole gate. This needs
authored/public tests for release behavior and repeated-phrase consistency.
Do not discard the release state merely to regain capacity. Offset0xfffe is
independent of the device's256KB whole-MDX allocation; loop sharing is needed
before treating that allocation as the active limit.

Independent architect review agrees: track8 offset is cumulative preceding data, not proof that track8 itself is oversized. Whole-gate fallback/repeat fragmentation is plausible but unproven; preserve zero-target terminal KC/KF/TL in the existing renderer order instead of removing the release guard.
