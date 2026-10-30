# Nemesi95 normalization sweep (2026-10-23)

Read-only analysis of user-generated artifacts. All source hashes match across runs.
FM-state remains 24ms throughout. Baseline STATE24 uses normalization8ms.
User listening report: no perceived issue across the tested settings.
This is subjective listening evidence, not an independently measured equivalence claim.

| normalization ms | success | MDX bytes (24 songs) | MML bytes | encoded loop pairs | omitted whole FM gates |
|---:|---:|---:|---:|---:|---:|
| 8 | 24/25 | 275253 | 422992 | 2059 | 1 |
| 16 | 24/25 | 275081 | 422390 | 2048 | 121 |
| 24 | 24/25 | 275038 | 422239 | 2049 | 136 |
| 32 | 24/25 | 275205 | 422664 | 2049 | 161 |
| 64 | 24/25 | 279546 | 434674 | 1889 | 2414 |

## Interpretation

The best aggregate MDX size is at24ms, only215bytes (about0.078%) below8ms.
All8->16/24/32 runs change MDX size for only6 of the24 successful songs.
64ms increases aggregate MDX by4293bytes (about1.56%), with loop pairs2059->1889,
register writes16499->19544, encoded NOTE commands38697->37533 and
KeyOffDisable commands19222->19427. Counts describe stored commands, not loop-expanded performance.
Larger thresholds do not monotonically improve repeat structure or compiled size.
The data show fewer loops and more register writes at64ms; attributing each change to a
particular repeat-break mechanism would require phrase-level comparison.

Normalization is also a whole-gate omission cutoff; it is not just an internal-state smoother.
At64ms2414 source-derived FM gates are omitted versus1 at8ms. Native Segments remain intact,
and elapsed song time is retained; this does not mean the score is unchanged.
There is no significant size-based case for changing the default8ms from this set alone.

All settings retain the same24 successes. NEME9525 fails identically on rate_change_during_playback
plus irregular_byte_supply; normalization does not implement variable-rate PCM projection.
The five successful PCM-bearing outputs have identical PDX hashes across all settings.

## Artifacts

outputs/diagnostics/nemesi95_normalization_sweep_20261023/summary.json
outputs/diagnostics/nemesi95_normalization_sweep_20261023/per_song.csv
outputs/diagnostics/nemesi95_normalization_sweep_20261023/verification.json

No source/test/default changes; no private-source reconversion or independent listening.
