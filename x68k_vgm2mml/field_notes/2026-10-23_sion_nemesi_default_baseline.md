# User regression: default baseline after auto/mixed-supply correction

The user supplied results without a normalization sweep. Existing results and
assessment/normalization JSON were inspected read-only; no local_only source
was reconverted and no listening validation was performed here.

- Sion II: 7/8 success, one compilation_failed (SIONII06).
- Nemesi95: 24/25 success, one pcm_projection_blocked (NEME9525).
- Combined: 31/33, compared with the earlier 19/33 (0/8 and 19/25).
- Command leaves FM-state and normalization unspecified. SIONII06 pass JSON
  confirms threshold_ms=24, threshold_samples=1058 and normalization_ms=8.
- Actual DAC supplies select auto -> best-effort in the affected PCM cases.

## Remaining failures

SIONII06: source bank validation succeeds, one sample/playback is retained,
490093 payload bytes / 490861 PDX bytes including table, zero playback omissions.
Compilation fails with MDX track 8 offset exceeding 0xfffe. This is an MDX
track-start offset constraint, not a PDX byte-cap failure. EX-PDX does not relax
it; the fact that the MDX allocation ceiling is 256KB does not remove the
individual track-start offset restriction. MML and PDX are retained for inspection.

NEME9525: native PCM issues=[], but playback 0 has both
rate_change_during_playback and irregular_byte_supply. The continuous-delivery
fallback accepts only irregular_byte_supply, so best-effort correctly does not
claim support for this additional speed-trajectory projection. This is a target
projection gap, not evidence of damaged VGM or PDX total-capacity overflow.
Retain decoder continuity and control times if extending this route; merely
removing the eligibility check would not implement variable-rate playback.

Successful output generation remains distinct from runtime memory fit and
musical equivalence. No new threshold decision or sweep was made.
