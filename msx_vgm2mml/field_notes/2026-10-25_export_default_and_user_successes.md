# MGS export default and user-reported successes (2026-10-25)

## Saved behavior

scripts/export_mgs.py now enables software_volume_curves by default. --no-software-volume-curves explicitly disables it; the two flags remain mutually exclusive. The canonical vgm2mml.py and legacy scripts/batch_vgm_to_mgs.py defaults remain disabled. MgsOptions continues forwarding an explicit enabled/disabled choice. README is user-maintained and was not edited.

## User regression successes only

These are success statuses extracted from the supplied root log files; no new conversion or playback validation was performed for this record. Success does not certify every-chip waveform equality. Failure details are intentionally not analyzed here.

### NEMESIS2: 9 successful inputs

Log: NEMESIS2_software-volume-curves.log

- GRA2_02.vgm
- GRA2_05.vgm
- GRA2_06.vgm
- GRA2_07.vgm
- GRA2_11.vgm
- GRA2_12.vgm
- GRA2_14.vgm
- GRA2_15.vgm
- GRA2_17.vgm

### gra2_msx: 47 successful inputs

Log: gra2_msx_software-volume-curves.log

- gra2_001/with_sync_mark/gra2_001_with_sync_mark01.vgm
- gra2_001/with_sync_mark/gra2_001_with_sync_mark02.vgm
- gra2_001/with_sync_mark/gra2_001_with_sync_mark03.vgm
- gra2_001/with_sync_mark/gra2_001_with_sync_mark04.vgm
- gra2_001/with_sync_mark/gra2_001_with_sync_mark05.vgm
- gra2_001/with_sync_mark/gra2_001_with_sync_mark06.vgm
- gra2_002/gra2_002.vgm
- gra2_003/with_sync_mark/gra2_003_with_sync_mark01.vgm
- gra2_003/with_sync_mark/gra2_003_with_sync_mark02.vgm
- gra2_004/with_sync_mark/gra2_004_with_sync_mark01.vgm
- gra2_004/with_sync_mark/gra2_004_with_sync_mark02.vgm
- gra2_004/with_sync_mark/gra2_004_with_sync_mark03.vgm
- gra2_005/gra2_005.vgm
- gra2_005/with_sync_mark/gra2_005_with_sync_mark01.vgm
- gra2_006/gra2_006.vgm
- gra2_006/with_sync_mark/gra2_006_with_sync_mark01.vgm
- gra2_007/with_sync_mark/gra2_007_with_sync_mark01.vgm
- gra2_007/with_sync_mark/gra2_007_with_sync_mark02.vgm
- gra2_007/with_sync_mark/gra2_007_with_sync_mark03.vgm
- gra2_008/gra2_008.vgm
- gra2_008/with_sync_mark/gra2_008_with_sync_mark01.vgm
- gra2_009/with_sync_mark/gra2_009_with_sync_mark01.vgm
- gra2_009/with_sync_mark/gra2_009_with_sync_mark02.vgm
- gra2_010/with_sync_mark/gra2_010_with_sync_mark01.vgm
- gra2_010/with_sync_mark/gra2_010_with_sync_mark02.vgm
- gra2_011/with_sync_mark/gra2_011_with_sync_mark01.vgm
- gra2_011/with_sync_mark/gra2_011_with_sync_mark02.vgm
- gra2_011/with_sync_mark/gra2_011_with_sync_mark03.vgm
- gra2_012/gra2_012.vgm
- gra2_012/with_sync_mark/gra2_012_with_sync_mark01.vgm
- gra2_013/gra2_013.vgm
- gra2_013/with_sync_mark/gra2_013_with_sync_mark01.vgm
- gra2_014/with_sync_mark/gra2_014_with_sync_mark01.vgm
- gra2_014/with_sync_mark/gra2_014_with_sync_mark02.vgm
- gra2_014/with_sync_mark/gra2_014_with_sync_mark03.vgm
- gra2_014/with_sync_mark/gra2_014_with_sync_mark04.vgm
- gra2_014/with_sync_mark/gra2_014_with_sync_mark05.vgm
- gra2_014/with_sync_mark/gra2_014_with_sync_mark06.vgm
- gra2_014/with_sync_mark/gra2_014_with_sync_mark07.vgm
- gra2_015/with_sync_mark/gra2_015_with_sync_mark01.vgm
- gra2_015/with_sync_mark/gra2_015_with_sync_mark02.vgm
- gra2_015/with_sync_mark/gra2_015_with_sync_mark03.vgm
- gra2_015/with_sync_mark/gra2_015_with_sync_mark04.vgm
- gra2_016/with_sync_mark/gra2_016_with_sync_mark01.vgm
- gra2_016/with_sync_mark/gra2_016_with_sync_mark02.vgm
- gra2_017/gra2_017.vgm
- gra2_017/with_sync_mark/gra2_017_with_sync_mark01.vgm

## Minimal verification

Three mocked export-default/forwarding/adjacent-output tests and eight configuration tests passed. No new fixture conversions were run. Focused git diff --check passed. Public code and private note are saved but uncommitted.
