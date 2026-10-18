# SegaPCM finite export results with timeout 600 (2026-10-14)

## Scope and evidence

User-reported regression results only. No additional artifact inspection,
conversion, compilation or listening test was performed for this record.
Investigation remains stopped at the user's request; only this note was added.
Related prior diagnosis: 2026-10-13_segapcm_pdx_regression_triage.md.

## Commands

Input: tests/fixtures/local_only/opm_segapcm/vgmrips.net/TBLADE/

Common options: --no-vgm --pcm-policy best-effort --compiler mmlx --no-song-loop

1. Default timeout (180 seconds):
   Output: outputs/opm_segapcm/vgmrips.net/TBLADE_NO_LOOP/mmlx
   Result: 7/12 exported.
2. Additional option: --timeout 600
   Output: outputs/opm_segapcm/vgmrips.net/TBLADE_NO_LOOP_TIMEOUT600/mmlx
   Result: 11/12 exported.

Both output directories contain results.csv according to the user's report.
The preceding TBOUR_NO_LOOP/mmlx report remains 4/8 exported; this report
provides no new TBOUR per-track results.

## Per-track results

Track   Default timeout        Timeout 600
TBL01   conversion_timeout     success
TBL02   success                success
TBL03   success                success
TBL04   conversion_timeout     success
TBL05   success                success
TBL06   success                success
TBL07   success                success
TBL08   success                success
TBL09   conversion_timeout     success
TBL10   success                success
TBL11   conversion_timeout     pcm_projection_blocked
TBL12   conversion_timeout     success

## Interpretation and limits

- TBL02 and TBL10 succeed with --no-song-loop. This bypasses the song-loop
  construction failures recorded in the prior loop-enabled run; it does not
  establish that loop-enabled construction has been fixed.
- TBL01, TBL04, TBL09 and TBL12 succeed when the timeout is increased to 600.
  The 180-second conversion budget was insufficient for these runs. No exact
  runtime profile or successful-run elapsed time was supplied.
- TBL11 advances from conversion_timeout to pcm_projection_blocked. The
  blocking PCM projection reason cannot be determined from these status lines;
  no diagnosis of the specific guard or underlying cause was performed.
- Export success confirms file generation, not native playback quality or
  source/target musical equivalence. This report contains no new listening
  result.

## Decisions

Keep the default timeout at 180 seconds. No code changes, additional tests,
new listening patterns, staging, commits or pushes were performed. Further
investigation or implementation requires the user's instruction to resume.
