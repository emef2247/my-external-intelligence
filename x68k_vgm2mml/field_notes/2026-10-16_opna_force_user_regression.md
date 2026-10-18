# OPNA -> MDX force user regression (2026-10-16)

User supplied terminal regression results. Assistant only read the attachment,
existing results.csv/native diagnostic JSON and staged VGM headers; no conversion,
test, compiler execution, playback or code modification. Success means exported
artifacts, not source equivalence or native listening confirmation.

Options reported: --no-vgm --pcm-policy best-effort --compiler mmlx --force.
Timeout failures in the existing CSVs explicitly use180 seconds.

## Recorded results

| Set | Recorded songs | Success | conversion_failed | compilation_failed | conversion_timeout |
|---|---:|---:|---:|---:|---:|
| GRSEED | 4 | 0 | 3 | 0 | 1 |
| ACTRAISER_MML | 4 | 4 | 0 | 0 | 0 |
| STCRU | 25 | 12 | 0 | 4 | 9 |
| XANADO | 20 | 13 | 0 | 4 | 3 |

GRSEED and STCRU were manually interrupted; their counts cover only the logged
completed per-song statuses, not all source files. KeyboardInterrupt tracebacks
are user interruption, not additional failed songs. ACTRAISER4/4 and XANADO13/20
have complete exporter summaries. Shell completion/directory listings are not
additional conversion runs.

## Causes confirmed from saved results

- GRSEED01/03/04: `Unresolved PCM source controls:
  unsupported_clock_or_chip_configuration`. Native PCM JSON clock_raw/clock_hz
  are both7987200, ordinary positive clock. Their saved staged VGM headers all
  have extra-header offset0x4. `opna_adpcm.analyze` marks any nonzero extra header
  unsupported; therefore the clock value itself is not the failed condition.
  `opna_force_pcm.prepare` does not whitelist this combined configuration code,
  so it remains blocking rather than becoming a whole-PCM omission. No claim is
  made that the extra-header content is safe to ignore or that PCM capacity failed.
- STCRU02/17: MDX track6 offset exceeds0xfffe; STCRU20/22: track5.
- XANADO08: track4 offset exceeds0xfffe;12: track7;15: track8;18: track3.
  All eight compilation failures are track-start offset capacity errors.
  These remain outside current compiler fallback scope.
- GRSEED02; STCRU01/05/06/10/11/14/18/23/25;
  XANADO06/14/16: conversion timed out after180 seconds. Existing terminal/CSV
  evidence does not establish the internal stage or that600 seconds would solve it.

ACTRAISER capacity summaries retain14 samples forgh002,5 forgh011,28 forstg001,
with0 capacity-omitted samples/playbacks. No capacity line is present forgh029;
do not infer an omitted count for that song. Unsupported rhythm/SSG/CH3 feature
lists are loss decisions, not by themselves the reason for a failed status.

Next: user controls regression/listening. No automatic retries, timeout-default
changes or projection fixes were performed or requested by this results report.
