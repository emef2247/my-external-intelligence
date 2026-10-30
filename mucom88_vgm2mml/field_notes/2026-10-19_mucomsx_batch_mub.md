# MUCOMSX batch MUB checkpoint — 2026-10-19

The user requested bulk MUCOMSX MUB generation for their regression. Their
command omitted --target mucomsx and used the default OPNA/MUCOM route,
causing explicit SCC omissions. No private song regression was performed here.

## Saved implementation

- export_mucom.py now compiles --target mucomsx with the existing local runtime
  profile by default; --runtime-profile selects another profile. --muc-only
  remains available without DOS assets.
- MUCOMSX compilation uses MUC2MUB only, in a fresh copied DOS image per song.
  It does not play music or export VGM. The standalone runtime still defaults
  to full compile/session/VGM checks; --compile-only selects the new mode.
- Generated MUC, dependencies and successful MUB remain in tracks/. Source
  Segments, projection/structure dumps, logs and DOS job evidence remain in
  per-run diagnostics. Failed/partial MUBs are never published.
- The batch timeout applies to each stage and overrides the runtime profile.

## Native environment findings

The supplied Nextor image runs COMMAND2 2.44. CALL RUN.BAT was unrecognized.
Invoking RUN.BAT directly started compilation, but its GOTO flow did not apply:
a valid CHECK.MUB and DONE.OK could be followed by an empty FAILED.ERR. The
interpreter help identifies GOTO/GOSUB as BTM commands. Staging now generates
RUN.BTM and AUTOEXEC.BAT invokes it directly. Cleanup removes both legacy BAT
and current BTM names from only the working disk copy. General DOS2/Nextor
compatibility must not be inferred from this COMMAND2-specific verification.

## Validation

52 focused batch/runtime/staging/CLI/public-reporter checks passed. The actual
native batch compiled DCSGDUAL, OPLLSRC, PSGSRC and SCCSRC (4/4). MUB byte sizes:
2633, 2483, 2721 and 1626. Signatures: msPb0100 for the first three, msPb0101
for SCCSRC. Each job exported only CHECK.MUB and DONE.OK. Source Segment CSVs
were inspected for PSG/OPLL/SCC (42/14/22 rows). Final MUB signature, persisted
completion markers and stable hashes are verified; this is not acoustic or
full MUB semantic validation.

Two authored negative DOS jobs (malformed loop MUC and missing compiler input)
timed out without DONE.OK or generated artifacts. They remained failed and did
not publish MUBs. These observations do not prove native IF ERRORLEVEL/GOTO
failure branching; mocked marker/publication tests cover the host behavior.

The existing mucomDotNET compiler-adapter mock tests in test_export_mucom
remain incompatible with WSL wslpath interception (three test methods, six
subtest failures and two errors). That unrelated adapter/test was not changed;
the module's 15 batch tests pass independently and are included in the 52.

Public artifacts: outputs/mucomsx_batch_mub_btm_20261019. Earlier startup
investigation outputs remain separate and must not be mistaken for final
results. No OPLL voice mapping, source interpretation or musical conversion
behavior changed. No Git stage, commit or push was performed in either repo.
Next action: user private regression; do not resume OPLL or enhancer changes.

## User regression log follow-up

Read-only inspection, no song rerun: gra2_001/run_cngzmvw_ failed before DOS
with a 227668/65536-byte MUC capacity check. Sync-mark01/run_fotvr4di and
sync-mark02/run_5v9pi7q4 both staged the obsolete AUTOEXEC containing
@ECHO OFF and CALL RUN.BAT, plus RUN.BAT rather than RUN.BTM. Their working
disks contain no allowlisted generated files. These jobs used the pre-fix
launcher; the current source directly launches RUN.BTM. A Python batch started
before the edits retains already-imported runtime functions across songs.
Restart the batch process to load the corrected launcher. The MUC capacity
failure remains independent and is not fixed by restarting. No active matching
user batch process was seen at inspection. No code/test changes or private
regression were performed for this follow-up.
