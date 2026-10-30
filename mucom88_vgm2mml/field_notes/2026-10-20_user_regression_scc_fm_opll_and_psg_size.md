# User regression results — 2026-10-20

Recording only at the user's request (1% allowance remaining). No source,
conversion, compiler, playback or additional diagnostic tests were run.
Raw user-provided terminal output is preserved below. Success means reported
conversion/MUB generation, not a new listening-equivalence claim.

## Reported results

- Target: mucomsx with --force, after all SCC source voices were switched to
  OPNA FM and the two OPLL rhythm-loop errors were corrected.
- PSG+SCC gra2_msx: 36/58 entries successful; the others in this
  pasted run report MUC input capacity over 65536 bytes. Whole songs and split
  sync-mark excerpts are separate entries. p52_arbl: 853686/65536 bytes.
- OPLL F1SPRT3D: all 11 successful. Previously reported rhythm loop-boundary
  errors and bare source-sample exceptions no longer appear in this report.
- OPLL FMPAC: all five successful, reported twice; the second run has a
  terminal typo before FMPAC02 but still reports success.
- OPLL ALESTE2: 9/17 successful; 02/04/08/10/12/14/15/16 exceed MUC capacity.
- OPLL grider: 92104/65536 bytes; sx01v: 81660/65536 bytes.
- PSG+OPLL es56: 531456/65536 bytes. User's MSX/MGS es56 MML is about 94K,
  with an MGSC track-buffer error. Preserve this as a separate named case;
  it is not the same input as grider.
- PSG+OPLL FFMSX: 07 and 12 successful; 03 refuses to replace an unowned MUC;
  the other 11 entries exceed MUC input capacity.
- User's MSX/MGS FFMSX run succeeds for 01/02/03/05/06/07/08/09/11/12/13;
  04 and 10 report MGSC buffer_error. No 14 result is shown in that excerpt.
- Key discrepancy: FFMSX01 is about 7.8K MGS MML (6.0K compiled MGS), while
  MUCOMSX candidate is 130853 bytes and rejected. These text-size observations
  are user measurements; no causal explanation has been established here.

## Explicit next task after allowance reset

Inspect PSG-to-MUC rendering/structure and compare intermediate source units,
volume runs, loop/macro output for FFMSX01 against msx_vgm2mml. Do not assume
all current size failures are inherent MUCOMSX limits. The user specifically
asks for that inspection after reset, not now. Preserve source evidence and
musical behavior; do not bypass the capacity check or discard control data.
Ownership refusal is a separate failure category; do not delete/overwrite it.
Continue user-owned local_only regression policy. Do not start optimization,
reconversion, code changes, or listening tests during this recording turn.

No Git stage, commit or push was performed.

## Original user report

```text
リグレッションの結果を記録します。残り1%しかないので、とりあえず記録してください。


psg_sccの変換
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/psg_scc/msxplay.com/p52_arbl/ --outdir outputs/msxplay.com/p52_arbl/ --force --target mucomsx
p52_arbl.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 853686/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/msxplay.com/p52_arbl/_diagnostics/p52_arbl/run_rmlo_2jf/report.txt

emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/psg_scc/msxplay.com/gra2_msx --outdir outputs/psg_scc/msxplay.com/gra2_msx --force --target mu
comsx
gra2_001/gra2_001.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 266198/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_001/run_jc56by0i/report.txt
gra2_001/with_sync_mark/gra2_001_with_sync_mark01.vgm: success
gra2_001/with_sync_mark/gra2_001_with_sync_mark02.vgm: success
gra2_001/with_sync_mark/gra2_001_with_sync_mark03.vgm: success
gra2_001/with_sync_mark/gra2_001_with_sync_mark04.vgm: success
gra2_001/with_sync_mark/gra2_001_with_sync_mark05.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 65953/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_001_with_sync_mark05/run_dwj7fpae/report.txt
gra2_001/with_sync_mark/gra2_001_with_sync_mark06.vgm: success
gra2_002/gra2_002.vgm: success
gra2_003/gra2_003.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 238119/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_003/run_e1iz_17m/report.txt
gra2_003/with_sync_mark/gra2_003_with_sync_mark01.vgm: success
gra2_003/with_sync_mark/gra2_003_with_sync_mark02.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 92764/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_003_with_sync_mark02/run_lsn2k8g0/report.txt
gra2_004/gra2_004.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 268955/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_004/run_1k5q3k85/report.txt
gra2_004/with_sync_mark/gra2_004_with_sync_mark01.vgm: success
gra2_004/with_sync_mark/gra2_004_with_sync_mark02.vgm: success
gra2_004/with_sync_mark/gra2_004_with_sync_mark03.vgm: success
gra2_005/gra2_005.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 68799/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_005/run_bab65mm1/report.txt
gra2_005/with_sync_mark/gra2_005_with_sync_mark01.vgm: success
gra2_006/gra2_006.vgm: success
gra2_006/with_sync_mark/gra2_006_with_sync_mark01.vgm: success
gra2_007/gra2_007.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 146932/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_007/run_x0yzoogw/report.txt
gra2_007/with_sync_mark/gra2_007_with_sync_mark01.vgm: success
gra2_007/with_sync_mark/gra2_007_with_sync_mark02.vgm: success
gra2_007/with_sync_mark/gra2_007_with_sync_mark03.vgm: success
gra2_008/gra2_008.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 76003/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_008/run_d2wnbzdc/report.txt
gra2_008/with_sync_mark/gra2_008_with_sync_mark01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 75566/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_008_with_sync_mark01/run_elm4z80p/report.txt
gra2_009/gra2_009.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 140296/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_009/run_p31rjl_7/report.txt
gra2_009/with_sync_mark/gra2_009_with_sync_mark01.vgm: success
gra2_009/with_sync_mark/gra2_009_with_sync_mark02.vgm: success
gra2_010/gra2_010.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 301790/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_010/run_k83z4r0v/report.txt
gra2_010/with_sync_mark/gra2_010_with_sync_mark01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 87958/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_010_with_sync_mark01/run_5kpo0mbj/report.txt
gra2_010/with_sync_mark/gra2_010_with_sync_mark02.vgm: success
gra2_011/gra2_011.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 188411/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_011/run_zbdqio2j/report.txt
gra2_011/with_sync_mark/gra2_011_with_sync_mark01.vgm: success
gra2_011/with_sync_mark/gra2_011_with_sync_mark02.vgm: success
gra2_011/with_sync_mark/gra2_011_with_sync_mark03.vgm: success
gra2_012/gra2_012.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 109662/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_012/run_a4342_ks/report.txt
gra2_012/with_sync_mark/gra2_012_with_sync_mark01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 109671/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_012_with_sync_mark01/run_othgo0zk/report.txt
gra2_013/gra2_013.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 114225/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_013/run_90j0kpis/report.txt
gra2_013/with_sync_mark/gra2_013_with_sync_mark01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 114243/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_013_with_sync_mark01/run_va24hajk/report.txt
gra2_014/gra2_014.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 271416/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_014/run_ejlcjnny/report.txt
gra2_014/with_sync_mark/gra2_014_with_sync_mark01.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark02.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark03.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark04.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark05.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark06.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark07.vgm: success
gra2_015/gra2_015.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 170085/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_015/run_9qwosw9i/report.txt
gra2_015/with_sync_mark/gra2_015_with_sync_mark01.vgm: success
gra2_015/with_sync_mark/gra2_015_with_sync_mark02.vgm: success
gra2_015/with_sync_mark/gra2_015_with_sync_mark03.vgm: success
gra2_015/with_sync_mark/gra2_015_with_sync_mark04.vgm: success
gra2_016/gra2_016.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 290966/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_016/run_ehv82c0p/report.txt
gra2_016/with_sync_mark/gra2_016_with_sync_mark01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 82491/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_016_with_sync_mark01/run_z_ecdjik/report.txt
gra2_016/with_sync_mark/gra2_016_with_sync_mark02.vgm: success
gra2_016/with_sync_mark/gra2_016_with_sync_mark03.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 135737/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_016_with_sync_mark03/run_bxv6ppw4/report.txt
gra2_017/gra2_017.vgm: success
gra2_017/with_sync_mark/gra2_017_with_sync_mark01.vgm: success

psg_opllの変換
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/psg_opll/msxplay.com/es56 --outdir outputs/psg_opll/msxplay.com/es56 --force --target mucomsx
es56.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 531456/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/msxplay.com/es56/_diagnostics/es56/run_2qn_8dn7/report.txt

opllのみの変換
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/opll/vgmrips.net/F1SPRT3D/ --outdir outputs/opll/vgmrips.net/F1SPRT3D/ --force --target mucomsx
F1SP3D01.vgm: success
F1SP3D02.vgm: success
F1SP3D03.vgm: success
F1SP3D04.vgm: success
F1SP3D05.vgm: success
F1SP3D06.vgm: success
F1SP3D07.vgm: success
F1SP3D08.vgm: success
F1SP3D09.vgm: success
F1SP3D10.vgm: success
F1SP3D11.vgm: success

emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/opll/msxplay.com/grider/ --outdir outputs/opll/msxplay.com/grider/ --force --target mucomsx
grider.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 92104/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/opll/msxplay.com/grider/_diagnostics/grider/run_24exbxv2/report.txt

emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/opll/msxplay.com/sx01v/ --outdir outputs/opll/msxplay.com/sx01v --force --target mucomsx
sx01v.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 81660/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/opll/msxplay.com/sx01v/_diagnostics/sx01v/run_0e8hurzy/report.txt

emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/opll/vgmrips.net/FMPAC --outdir outputs/opll/vgmrips.net/FMPAC --force --target mucomsx
FMPAC01.vgm: success
FMPAC02.vgm: success
FMPAC03.vgm: success
FMPAC04.vgm: success
FMPAC05.vgm: success

emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/opll/vgmrips.net/FMPAC --outdir outputs/opll/vgmrips.net/FMPAC --force --target mucomsx
FMPAC01.vgm: success
gFMPAC02.vgm: success
FMPAC03.vgm: success
FMPAC04.vgm: success
FMPAC05.vgm: success
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/opll/vgmrips.net/ALESTE2 --outdir outputs/opll/vgmrips.net/ALESTE2 --force --target mucomsx
Alest201.vgm: success
Alest202.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 157245/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/opll/vgmrips.net/ALESTE2/_diagnostics/Alest202/run_blgu8xpo/report.txt
Alest203.vgm: success
Alest204.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 184564/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/opll/vgmrips.net/ALESTE2/_diagnostics/Alest204/run_0o4fz443/report.txt
Alest205.vgm: success
Alest206.vgm: success
Alest207.vgm: success
Alest208.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 309946/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/opll/vgmrips.net/ALESTE2/_diagnostics/Alest208/run_8zjzllg3/report.txt
Alest209.vgm: success
Alest210.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 150427/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/opll/vgmrips.net/ALESTE2/_diagnostics/Alest210/run_ms6w_738/report.txt
Alest211.vgm: success
Alest212.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 66487/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/opll/vgmrips.net/ALESTE2/_diagnostics/Alest212/run_vicx_nvo/report.txt
Alest213.vgm: success
Alest214.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 107720/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/opll/vgmrips.net/ALESTE2/_diagnostics/Alest214/run_0qlhv64j/report.txt
Alest215.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 126039/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/opll/vgmrips.net/ALESTE2/_diagnostics/Alest215/run_rv5xb5nf/report.txt
Alest216.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 74685/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/opll/vgmrips.net/ALESTE2/_diagnostics/Alest216/run_sw_o27rn/report.txt
Alest217.vgm: success

psg_opll:
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/psg_opll/vgmrips.net/FFMSX/ --outdir outputs/psg_opll/vgmrips.net/FFMSX/ --target mucomsx --force
FFMSX01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 130853/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX01/run_tpl2ibp3/report.txt
FFMSX02.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 83119/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX02/run_xw89xegl/report.txt
FFMSX03.vgm: conversion_failed: Refusing to replace unowned artifact: /mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/tracks/FFMSX03/FFMSX03.muc
FFMSX04.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 176077/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX04/run_pf2mx6s1/report.txt
FFMSX05.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 71174/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX05/run_7kx6rqoh/report.txt
FFMSX06.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 86997/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX06/run_50tnnopg/report.txt
FFMSX07.vgm: success
FFMSX08.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 92778/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX08/run__mfxo55w/report.txt
FFMSX09.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 185751/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX09/run_6siy8jdc/report.txt
FFMSX10.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 135656/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX10/run_reswf8ts/report.txt
FFMSX11.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 169007/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX11/run_e852yp2n/report.txt
FFMSX12.vgm: success
FFMSX13.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 88346/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX13/run__u1_xe9t/report.txt
FFMSX14.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 145037/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX14/run_reth4czu/report.txt


ちなみにOPLLのみの曲でサイズオーバーになるのは

mucom88_vgm2mml:
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/opll/msxplay.com/grider/ --outdir outputs/opll/msxplay.com/grider/ --force --target mucomsx
grider.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 92104/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/opll/msxplay.com/grider/_diagnostics/grider/run_24exbxv2/report.txt

msx_vgm2mml:
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ python scripts/batch_vgm_to_mgs.py tests/fixtures/local_only/psg_opll/msxplay.com/es56 --outdir outputs/psg_opll/msxplay.com/es56
Compiler: /usr/local/bin/mgsc

MGSC MML compiler version 1.11
Copyright(c) 1992-94 by Ain

Track buffer full in 83
>> 1 c%21]6 [c16 c%9]4 n3 @e5 c%42 n11 @e1 c%21 c%21 n3 @e5 c%42 n11 @e1 c%42 n3 @e5 c%42 n11 @e1 c%21 c%21]2 n3 @e5 c%42

es56.vgm buffer_error
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ ls -lh outputs/psg_opll/msxplay.com/es56/*mml
ls: cannot access 'outputs/psg_opll/msxplay.com/es56/*mml': No such file or directory
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ ls -lh outputs/psg_opll/msxplay.com/es56/
es56.vgm/        keyon_setup.log  results.csv      results.json
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ ls -lh outputs/psg_opll/msxplay.com/es56/es56.vgm/es56.mml
-rwxr--r-- 1 emef220 emef220 94K Oct 20 04:16 outputs/psg_opll/msxplay.com/es56/es56.vgm/es56.mml

なので、MMLが90Kオーバーになるのは妥当だと思います。

一方でpsg_opllは全然違います。

msx_vgm2mmlで一括変換：
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ python scripts/batch_vgm_to_mgs.py tests/fixtures/local_only/psg_opll/vgmrips.net/FFMSX/ --outdir outputs/psg_opll/vgmrips.net/FFMSX/
Compiler: /usr/local/bin/mgsc
FFMSX01.vgm success
FFMSX02.vgm success
FFMSX03.vgm success

MGSC MML compiler version 1.11
Copyright(c) 1992-94 by Ain

Track buffer full in 117
>> 2 r%36 > \0 f%36]2 [< a+%36 > a+%36]4 \2 @e1 d%72 \4 c%72 < b%72 \5 a%72 \-1 @e3 g%36 g%36

FFMSX04.vgm buffer_error
FFMSX05.vgm success
FFMSX06.vgm success
FFMSX07.vgm success
FFMSX08.vgm success
FFMSX09.vgm success

MGSC MML compiler version 1.11
Copyright(c) 1992-94 by Ain

Track buffer full in 137
>> 9 b32 & *22 b32 & ) q8 b%15 q0 a+32 & *23 a+32 [& ) a+%21 & *23 a+32]15 & ) q8 a+%15 *11 b32 & ) q8 b%21

FFMSX10.vgm buffer_error
FFMSX11.vgm success
FFMSX12.vgm success
FFMSX13.vgm success

FFMSX01.vgm/で着目するとMMLのサイズは7.8K程度です。

emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ find outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/ -type f
outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/FFMSX01.mml
outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/convert.log
outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/FFMSX01.mgs
outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/compile.log
outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/FFMSX01.roundtrip.vgm
outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/keyon.log
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ find outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/ -type f | xargs ls -lh
-rwxr--r-- 1 emef220 emef220 6.0K Oct 20 04:20 outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/FFMSX01.mgs
-rwxr--r-- 1 emef220 emef220 7.8K Oct 20 04:20 outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/FFMSX01.mml
-rwxr--r-- 1 emef220 emef220 2.1K Oct 20 04:20 outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/FFMSX01.roundtrip.vgm
-rwxr--r-- 1 emef220 emef220   98 Oct 20 04:20 outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/compile.log
-rwxr--r-- 1 emef220 emef220  121 Oct 20 04:20 outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/convert.log
-rwxr--r-- 1 emef220 emef220   63 Oct 20 04:20 outputs/psg_opll/vgmrips.net/FFMSX/FFMSX01.vgm/keyon.log
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$

一方でmucom88_vgm2mmlでは６４Kを超えています。
枠が回復したら、psgをmml化する処理を点検しましょう。
```


## FFMSX full-run update — 2026-10-20 (record only)

Supersedes the earlier partial FFMSX coverage above. User allowance has NOT
reset; explicit request is to record only. No investigation or tests performed.

- MUCOMSX: 4/21 successful (FFMSX07.vgm, FFMSX12.vgm, FFMSX19.vgm, FFMSX20.vgm).
- MUCOMSX: 16 MUC capacity failures; 1 ownership refusal (FFMSX03.vgm).
- MSX/MGS: 15/21 successful; 6 MGSC buffer errors
  (FFMSX04.vgm, FFMSX10.vgm, FFMSX15.vgm, FFMSX16.vgm, FFMSX18.vgm, FFMSX21.vgm).
- Same-input size comparison FFMSX07: generated MUC about 44K vs MGS MML
  about 6.3K, approximately 6–7 times. FFMSX01 comparison remains about7.8K
  MGS MML vs 130853-byte rejected MUC.
- User suspects ineffective macro/envelope lowering. This remains a hypothesis;
  the pasted MUC already contains 32 macro definitions, repeated setup commands,
  explicit detune/octave/volume parameters and tied software-volume runs.
  Do not record the cause as proven or say macros are wholly absent.
- After reset, inspect PSG musical-unit grouping and structural/envelope/macro
  rendering using the source-linked intermediate artifacts, then compare target
  syntax costs. Preserve source behavior/evidence. No optimization is authorized
  to start during this record-only turn.
- All commands, individual results, compiler messages and MUC/MML excerpts from
  the attachment are preserved verbatim below; no private material enters the
  public handoff. No stage, commit or push.

### Original full-run update

```text
FFMSXの結果のアップデートです。全曲完了しました。枠は回復していません。記録だけしてください

mucom88_vgm2mml:
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/psg_opll/vgmrips.net/FFMSX/ --outdir outputs/psg_opll/vgmrips.net/FFMSX/ --target mucomsx --force
FFMSX01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 130853/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX01/run_tpl2ibp3/report.txt
FFMSX02.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 83119/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX02/run_xw89xegl/report.txt
FFMSX03.vgm: conversion_failed: Refusing to replace unowned artifact: /mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/tracks/FFMSX03/FFMSX03.muc
FFMSX04.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 176077/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX04/run_pf2mx6s1/report.txt
FFMSX05.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 71174/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX05/run_7kx6rqoh/report.txt
FFMSX06.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 86997/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX06/run_50tnnopg/report.txt
FFMSX07.vgm: success
FFMSX08.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 92778/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX08/run__mfxo55w/report.txt
FFMSX09.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 185751/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX09/run_6siy8jdc/report.txt
FFMSX10.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 135656/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX10/run_reswf8ts/report.txt
FFMSX11.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 169007/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX11/run_e852yp2n/report.txt
FFMSX12.vgm: success
FFMSX13.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 88346/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX13/run__u1_xe9t/report.txt
FFMSX14.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 145037/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX14/run_reth4czu/report.txt
FFMSX15.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 202878/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX15/run_un9bnzip/report.txt
FFMSX16.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 263684/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX16/run_cuz8ubty/report.txt
FFMSX17.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 89997/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX17/run_b2v58w43/report.txt
FFMSX18.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 159720/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX18/run_13rf1vtr/report.txt
FFMSX19.vgm: success
FFMSX20.vgm: success
FFMSX21.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 530539/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_opll/vgmrips.net/FFMSX/_diagnostics/FFMSX21/run_oyxg4_2z/report.txt

msx_vgm2mml:
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ python scripts/batch_vgm_to_mgs.py tests/fixtures/local_only/psg_opll/msxplay.com/es56 --outdir outputs/psg_opll/msxplay.com/es56
Compiler: /usr/local/bin/mgsc

MGSC MML compiler version 1.11
Copyright(c) 1992-94 by Ain

Track buffer full in 83
>> 1 c%21]6 [c16 c%9]4 n3 @e5 c%42 n11 @e1 c%21 c%21 n3 @e5 c%42 n11 @e1 c%42 n3 @e5 c%42 n11 @e1 c%21 c%21]2 n3 @e5 c%42

es56.vgm buffer_error
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ ls -lh outputs/psg_opll/msxplay.com/es56/*mml
ls: cannot access 'outputs/psg_opll/msxplay.com/es56/*mml': No such file or directory
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ ls -lh outputs/psg_opll/msxplay.com/es56/
es56.vgm/        keyon_setup.log  results.csv      results.json
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ ls -lh outputs/psg_opll/msxplay.com/es56/es56.vgm/es56.mml
-rwxr--r-- 1 emef220 emef220 94K Oct 20 04:16 outputs/psg_opll/msxplay.com/es56/es56.vgm/es56.mml
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ python scripts/batch_vgm_to_mgs.py tests/fixtures/local_only/psg_opll/vgmrips.net/FFMSX/ --outdir outputs/psg_opll/vgmrips.net/FFMSX/
Compiler: /usr/local/bin/mgsc
FFMSX01.vgm success
FFMSX02.vgm success
FFMSX03.vgm success

MGSC MML compiler version 1.11
Copyright(c) 1992-94 by Ain

Track buffer full in 117
>> 2 r%36 > \0 f%36]2 [< a+%36 > a+%36]4 \2 @e1 d%72 \4 c%72 < b%72 \5 a%72 \-1 @e3 g%36 g%36

FFMSX04.vgm buffer_error
FFMSX05.vgm success
FFMSX06.vgm success
FFMSX07.vgm success
FFMSX08.vgm success
FFMSX09.vgm success

MGSC MML compiler version 1.11
Copyright(c) 1992-94 by Ain

Track buffer full in 137
>> 9 b32 & *22 b32 & ) q8 b%15 q0 a+32 & *23 a+32 [& ) a+%21 & *23 a+32]15 & ) q8 a+%15 *11 b32 & ) q8 b%21

FFMSX10.vgm buffer_error
FFMSX11.vgm success
FFMSX12.vgm success
FFMSX13.vgm success
FFMSX14.vgm success

MGSC MML compiler version 1.11
Copyright(c) 1992-94 by Ain

Track buffer full in 141
>> 3 \0 *1 \1 ( *1 \0 *1 \-1 ( d+32 \19 ( d64 \18 d64 \19 ( d64 \-1 v12 d+32 \0 ( *1 \1 *1 \0 ( *1 \-1 @e6 v15 d+32 \19

FFMSX15.vgm buffer_error

MGSC MML compiler version 1.11
Copyright(c) 1992-94 by Ain

Track buffer full in 148
>> 3 \50 b64 \49 ( b64 \50 b64 > \0 c32 \1 c64 \2 ( c64 \1 c64 \0 c32 < \50 b64 \49 ( b64 \50 b64 > \0 c32 \1 c64 \2 ( c64

FFMSX16.vgm buffer_error
FFMSX17.vgm success

MGSC MML compiler version 1.11
Copyright(c) 1992-94 by Ain

Track buffer full in 137
>> 9 *20 a64]2 & *20 q8 a64 ( q0 a+64 & ( a+32 & ( a+32 & ( a+64 & ) a+32 & ) a+32 & ) a+64 & ) a+64 & ( a+64 & ( a+32 & (

FFMSX18.vgm buffer_error
FFMSX19.vgm success
FFMSX20.vgm success

MGSC MML compiler version 1.11
Copyright(c) 1992-94 by Ain

Track buffer full in 106
>> 3 \0 *1 *24 *2 \-1 ( *2 [\-1 *2]3 \-1 ( *2 [\-1 *2]5 *20 *1 \0 ( *1 [\0 *1]3 \0 ( *1 \0 *1 *16 \1 d+64 \0 d+64 *20 *1

FFMSX21.vgm buffer_error


MUCが生成されているFFMSX07.vgmでサイズ比較
mucom88_vgm2mml:
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ ls -lh outputs/psg_opll/vgmrips.net/FFMSX/tracks/FFMSX07/FFMSX07.muc
-rwxr--r-- 1 emef220 emef220 44K Oct 20 04:24 outputs/psg_opll/vgmrips.net/FFMSX/tracks/FFMSX07/FFMSX07.muc

msx_vgm2mml:
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ ls -lh outputs/psg_opll/vgmrips.net/FFMSX/FFMSX07.vgm/FFMSX07.mml
-rwxr--r-- 1 emef220 emef220 6.3K Oct 20 04:23 outputs/psg_opll/vgmrips.net/FFMSX/FFMSX07.vgm/FFMSX07.mml

mucom88_vgm2mmlが出力するMML(MUC)はmsx_vgm2mmlが出力するMML(.mml)の約6-7倍です。

恐らくですが、mucom88_vgm2mmlが出力するMMLはマクロ化やエンベロープ指定が上手くできていない可能性が考えられます。

head -n 100 outputs/psg_opll/vgmrips.net/FFMSX/tracks/FFMSX07/FFMSX07.muc
#title FFMSX07
#driver mucomDotNET
#voice FFMSX07.dat
# *1{E255,0,0,0,0,255 P1}
# *2{o5 v10}
# *3{o5 v9}
# *4{D-16}
# *5{@4 v11 o5}
# *6{D-32}
# *7{o5 v12}
# *8{D-48}
# *9{v13 o5}
# *10{D-8 o4 v12 d%8 & v11 d%9 & v10 d%9 & v9 d%9 & v8 d%9 & v7 d%9 & v6 d%27}
# *11{c%9 & v11 c%9 & v10 c%8 & v9 c%9 & v8 c%9 & v7 c%9 & v6 c%27}
# *12{D-8 o4 v12 e%8 & v11 e%9 & v10 e%9 & v9 e%9 & v8 e%9 & v7 e%9 & v6 e%27}
# *13{v12 b%8 & v11 b%9 & v10 b%9 & v9 b%9 & v8 b%9 & v7 b%9 & v6}
# *14{D0 o4 v12 g%8 & v11 g%9 & v10 g%9 & v9 g%9 & v8 g%9 & v7 g%9 & v6 g%27}
# *15{o5 v11 v10}
# *16{D-8 o4 v12}
# *17{v12 g%9 & v11 g%9 & v10}
# *18{o6 v10}
# *19{o5 v8}
# *20{c%8 & v11 c%9 & v10}
# *21{v13 o3}
# *22{v13 o4}
# *23{o6 v9 c%4}
# *24{v11 e%9 & v10}
# *25{@5 v12 o4}
# *26{o5 v11}
# *27{c%111 & c%111 & c%98}
# *28{o6 v9 c%5}
# *29{v11 a%9 & v10}
# *30{v12 b%8 & v11 b%9 & v10}
# *31{D0 o4 v12}
# *32{o4 v10}
MSXD C128 t243 q0 r%1 *1 *4 *7 e%8 *1 *6 *7 v11 e%5 *1 *8 *26 e%4 *1 *6 *15 e%5 *1 *4 *2 e%9 *1 D0 *2 e%4 *1
MSXD D16 *2 e%5 *1 D0 *2 e%4 *1 *4 *2 e%9 *1 *6 *2 e%4 *1 *8 *2 e%5 *1 *6 *2 e%4 *1 *4 *2 e%9 *1 D0 *2 e%5 *1
MSXD D16 *3 e%4 *1 D0 *3 e%5 *1 *4 *3 e%8 *1 *6 *3 e%5 *1 *8 *3 e%4 *1 *6 *3 e%5 *1 *4 *3 e%9 *1 D0 *3 e%4 *1
MSXD D16 *3 e%5 *1 D0 *3 e%4 *1 *4 *3 e%9 *1 *6 *3 e%4 *1 *8 *3 e%5 *1 *6 *3 e%4 *1 *4 *3 e%5 *1 *4 *7 g%4 *1
MSXD D0 *7 g%5 *1 D16 *7 v11 g%4 *1 D0 *26 g%5 *1 *4 *15 g%8 *1 *6 *2 g%5 *1 *8 *2 g%4 *1 *6 *2 g%5 *1 *4 *2
MSXD g%9 *1 D0 *2 g%4 *1 D16 *2 g%5 *1 D0 *2 g%4 *1 *4 *2 g%9 *1 *6 *2 g%4 *1 *8 *2 g%5 *1 *6 *3 g%4 *1 *4 *3
MSXD g%9 *1 D0 *3 g%5 *1 D16 *3 g%4 *1 D0 *3 g%5 *1 *4 *3 g%9 *1 *6 *3 g%4 *1 *8 *3 g%4 *1 *6 *3 g%5 *1 *4 *3
MSXD g%9 *1 D0 *3 g%4 *1 D16 *3 g%5 *1 D0 *3 g%4 *1 *4 *3 g%9 *1 *8 *7 d%5 *1 D-64 *7 d%4 *1 *8 *7 v11 d%4 *1
MSXD *4 *26 d%5 & v10 d%4 *1 D0 *2 d%5 *1 D16 *2 d%4 *1 D0 *2 d%5 *1 *4 *2 d%9 *1 *8 *2 d%4 *1 D-64 *2 d%4 *1
MSXD *8 *2 d%5 *1 *4 *2 d%9 *1 D0 *2 d%4 *1 D16 *2 d%5 *1 D0 *2 d%4 *1 *4 *3 d%9 *1 *8 *3 d%5 *1 D-64 *3 d%4
MSXD *1 *8 *3 d%4 *1 *4 *3 d%9 *1 D0 *3 d%5 *1 D16 *3 d%4 *1 D0 *3 d%5 *1 *4 *3 d%9 *1 *8 *3 d%4 *1 D-64 *3
MSXD d%4 *1 *8 *3 d%5 *1 *4 *3 d%9 *1 D0 *3 d%4 r%1 *1 D16 *7 e%4 *1 D0 *7 e%4 *1 *4 *7 v11 e%9 *1 *6 *15 e%5
MSXD *1 *8 *2 e%4 *1 *6 *2 e%4 *1 *4 *2 e%9 *1 D0 *2 e%5 *1 D16 *2 e%4 *1 D0 *2 e%5 *1 *4 *2 e%9 *1 *6 *2 e%4
MSXD *1 *8 *2 e%5 *1 *6 *2 e%4 *1 *4 *2 e%4 r%1 *1 *4 *7 f%4 *1 D0 *7 f%4 *1 D16 *7 v11 f%5 *1 D0 *26 f%4 *1
MSXD *4 *15 f%9 *1 *6 *2 f%5 *1 *8 *2 f%4 *1 *6 *2 f%4 *1 *4 *2 f%9 *1 D0 *2 f%5 *1 D16 *2 f%4 *1 D0 *2 f%5 *1
MSXD *4 *2 f%9 *1 *6 *2 f%4 *1 *8 *2 f%5 *1 *6 *7 e%4 *1 *4 *7 e%4 & v11 e%5 *1 D0 *26 e%4 *1 D16 *15 e%5 *1
MSXD D0 *2 e%4 *1 *4 *2 e%9 *1 *6 *2 e%5 *1 *8 *2 e%4 *1 *6 *2 e%5 *1 *4 *2 e%8 *1 D0 *2 e%5 *1 D16 *2 e%4 *1
MSXD D0 *2 e%5 *1 *4 *2 e%9 *1 *8 *7 d%4 *1 D-64 *7 d%5 *1 *8 *7 v11 d%4 *1 *4 *26 d%5 & v10 d%4 *1 D0 *2 d%4
MSXD *1 D16 *2 d%5 *1 D0 *2 d%4 *1 *4 *2 d%9 *1 *8 *2 d%5 *1 D-64 *2 d%4 *1 *8 *2 d%5 *1 *4 *2 d%8 *1 D0 *2
MSXD d%5 *1 D16 *2 d%4 *1 D0 *2 d%5 *1 *4 *7 c%9 *1 *6 *7 v11 c%4 *1 *8 *26 c%5 *1 *6 *15 c%4 *1 *4 *2 c%9 *1
MSXD D0 *2 c%4 *1 D16 *2 c%5 *1 D0 *2 c%4 *1 *4 *2 c%9 *1 *6 *2 c%5 *1 *8 *2 c%4 *1 *6 *2 c%5 *1 *4 *2 c%8 *1
MSXD D0 *2 c%5 *1 D16 *3 c%4 *1 D0 *3 c%5 *1 *4 *3 c%9 *1 *6 *3 c%4 *1 *8 *3 c%5 *1 *6 *3 c%4 *1 *4 *3 c%9 *1
MSXD D0 *3 c%4 *1 D16 *3 c%5 *1 D0 *3 c%4 *1 *4 *3 c%9 *1 *6 *3 c%5 *1 *8 *3 c%4 *1 *6 *3 c%5 *1 *4 *3 c%4 v8
MSXD *1 *4 *19 c%5 *1 D0 *19 c%4 *1 D16 *19 c%4 *1 D0 *19 c%5 *1 *4 *19 c%9 *1 *6 *19 c%4 *1 *8 *19 c%5 *1 *6
MSXD *19 c%4 *1 *4 *19 c%9 *1 D0 *19 c%4 *1 D16 *19 c%5 *1 D0 *19 c%4 *1 *4 *19 c%9 *1 *6 *19 c%5 *1 *8 *19
MSXD c%4 r%1 *1 *6 *3 c%4 *1 *4 *3 c%9 *1 D0 *3 c%4 *1 D16 *3 c%4 *1 D0 *3 c%5 *1 *4 *3 c%9 *1 *6 *3 c%4 r%1
MSXD *1 *8 *2 c%4 *1 *6 *2 c%4 *1 *4 *2 c%9 *1 D0 *2 c%5 *1 D16 *2 c%4 *1 D0 *2 c%4 *1 *4 *2 c%9 r%1 *1 *6 *7
MSXD c%4 *1 *8 *7 c%4 *1 *6 *7 v11 c%5 *1 *4 *26 c%4 & v10 c%5 *1 D0 *2 c%4 *1 D16 *2 c%4 *1 D0 *2 c%5 *1 *4
MSXD *2 c%9 *1 *6 *2 c%4 *1 *8 *2 c%5 *1 *6 *2 c%4 *1 *4 *2 c%9 *1 D0 *2 c%5 *1 D16 *2 c%4 *1 D0 *2 c%4 r%1 *1
MSXD *4 *3 c%8 *1 *6 *3 c%5 *1 *8 *3 c%4 *1 *6 *3 c%5 *1 *4 *3 c%9 *1 D0 *3 c%4 *1 D16 *3 c%4 *1 D0 *3 c%5 *1
MSXD *4 *3 c%9 *1 *6 *3 c%4 *1 *8 *3 c%5 *1 *6 *3 c%4 *1 *4 *3 c%9 *1 D0 *3 c%5 *1 D16 *7 a%4 *1 D0 *7 a%4 *1
MSXD *4 *7 a%1 & v11 a%8 *1 *6 *15 a%5 *1 *8 *2 a%4 *1 *6 *2 a%5 *1 *4 *2 a%9 *1 D0 *2 a%4 *1 D16 *2 a%5 *1 D0
MSXD *2 a%4 *1 *4 *2 a%9 *1 *6 *2 a%4 *1 *8 *2 a%5 *1 *6 *2 a%4 *1 *4 *2 a%5 v9 *1 *4 *3 a%4 *1 D0 *3 a%5 *1
MSXD D16 *3 a%4 *1 D0 *3 a%4 *1 *4 *3 a%9 *1 *6 *3 a%5 *1 *8 *3 a%4 *1 *6 *3 a%5 *1 *4 *3 a%9 *1 D0 *3 a%4 *1
MSXD D16 *3 a%5 *1 D0 *3 a%4 *1 *4 *3 a%9 *1 *6 *3 a%4 *1 *8 *3 a%5 *1 *6 *3 a%4 *1 *4 *3 a%9 *1 D0 *3 a%5 *1
MSXD D16 *3 a%4 *1 D0 *3 a%5 *1 *4 *3 a%8 *1 *6 *3 a%5 *1 *8 *2 a%4 *1 *6 *2 a%5 *1 *4 *2 a%9 *1 D0 *2 a%4 *1
MSXD D16 *2 a%5 *1 D0 *2 a%4 *1 *4 *2 a%9 r%1 *1 *6 *7 a%3 *1 *8 *7 a%5 *1 *6 *7 v11 a%4 *1 *4 *26 a%5 & v10
MSXD a%4 *1 D0 *2 a%5 *1 D16 *2 a%4 *1 D0 *2 a%5 *1 *4 *2 a%8 *1 *6 *2 a%5 *1 *8 *2 a%4 *1 *6 *2 a%5 *1 *4 *2
MSXD a%9 *1 D0 *2 a%4 *1 D16 *2 a%5 *1 D0 *2 a%4 *1 *4 *7 g%9 *1 *6 *7 v11 g%4 *1 *8 *26 g%5 *1 *6 *15 g%4 *1
MSXD *4 *2 g%9 *1 D0 *2 g%5 *1 D16 *2 g%4 *1 D0 *2 g%5 *1 *4 *2 g%9 *1 *6 *2 g%4 *1 *8 *2 g%4 *1 *6 *2 g%5 *1
MSXD *4 *2 g%9 *1 D0 *2 g%4 *1 D16 *7 d%5 *1 D0 *7 d%4 *1 *4 *7 v11 d%9 *1 *8 *15 d%4 *1 D-64 *2 d%5 *1 *8 *2
MSXD d%4 *1 *4 *2 d%9 *1 D0 *2 d%5 *1 D16 *2 d%4 *1 D0 *2 d%5 *1 *4 *2 d%8 *1 *8 *2 d%5 *1 D-64 *2 d%4 *1 *8
MSXD *2 d%5 *1 *4 *2 d%4 r%1 *1 *4 *7 e%4 *1 D0 *7 e%4 *1 D16 *7 v11 e%5 *1 D0 *26 e%4 *1 *4 *15 e%9 *1 *6 *2
MSXD e%4 *1 *8 *2 e%5 *1 *6 *2 e%4 *1 *4 *2 e%9 *1 D0 *2 e%5 *1 D16 *2 e%4 *1 D0 *2 e%5 *1 *4 *2 e%9 *1 *6 *2
MSXD e%4 *1 *8 *2 e%4 r%1 *1 *6 *7 f%4 *1 *4 *7 f%4 & v11 f%5 *1 D0 *26 f%4 *1 D16 *15 f%5 *1 D0 *2 f%4 *1 *4
MSXD *2 f%9 *1 *6 *2 f%5 *1 *8 *2 f%4 *1 *6 *2 f%4 *1 *4 *2 f%9 *1 D0 *2 f%5 *1 D16 *2 f%4 *1 D0 *2 f%5 *1 *4
MSXD *2 f%9 *1 *6 *7 e%4 *1 *8 *7 e%4 *1 *6 *7 e%1 & v11 e%4 *1 *4 *26 e%4 & v10 e%5 *1 D0 *2 e%4 *1 D16 *2
MSXD e%5 *1 D0 *2 e%4 *1 *4 *2 e%9 *1 *6 *2 e%5 *1 *8 *2 e%4 *1 *6 *2 e%4 *1 *4 *2 e%9 *1 D0 *2 e%5 *1 D16 *2
MSXD e%4 *1 D0 *2 e%5 *1 *4 *3 e%9 *1 *6 *3 e%4 *1 *8 *3 e%5 *1 *6 *3 e%4 *1 *4 *3 e%9 *1 D0 *3 e%4 *1 D16 *3
MSXD e%5 *1 D0 *3 e%4 *1 *4 *3 e%9 *1 *6 *3 e%5 *1 *8 *3 e%4 *1 *6 *3 e%5 *1 *4 *3 e%8 *1 D0 *3 e%5 *1 D16 *19
MSXD e%4 *1 D0 *19 e%5 *1 *4 *19 e%9 *1 *6 *19 e%4 *1 *8 *19 e%5 *1 *6 *19 e%4 *1 *4 *19 e%9 *1 D0 *19 e%4 *1
MSXD D16 *19 e%5 *1 D0 *19 e%4 *1 *4 *19 e%9 *1 *6 *19 e%5 *1 *8 *19 e%4 *1 *6 *19 e%5 *1 *4 *19 e%4 *1 *4 *3
MSXD e%4 *1 D0 *3 e%5 *1 D16 *3 e%4 *1 D0 *3 e%5 *1 *4 *3 e%9 *1 *6 *3 e%4 *1 *8 *3 e%5 *1 *6 *3 e%4 *1 *4 *3
MSXD e%9 *1 D0 *3 e%4 *1 D16 *3 e%5 *1 D0 *3 e%4 *1 *4 *3 e%9 *1 *6 *3 e%5 *1 *8 *3 e%4 *1 *6 *3 e%5 *1 *4 *3
MSXD e%8 *1 D0 *3 e%5 *1 D16 *3 e%4 *1 D0 *3 e%5 *1 *4 *3 e%9 *1 *6 *3 e%4 *1 *8 *3 e%5 *1 *6 *3 e%4 *1 *4 *3
MSXD e%9 *1 D0 *3 e%4 *1 D16 *3 e%5 *1 D0 *3 e%4 *1 *4 *3 e%9 r%1 *1 *6 *2 e%4 *1 *8 *2 e%4 *1 *6 *2 e%5 *1 *4
MSXD *2 e%8 *1 D0 *2 e%5 *1 D16 *2 e%4 *1 D0 *2 e%5 *1 *4 *2 e%9 *1 *6 *2 e%4 *1 *8 *2 e%5 *1 *6 *2 e%4 *1 *4
MSXD *2 e%9 *1 D0 *2 e%4 *1 D16 *2 e%5 *1 D0 *2 e%4 r%1 *1 *4 *7 e%8 *1 *6 *7 v11 e%5 *1 *8 *26 e%4 *1 *6 *15
MSXD e%5 *1 *4 *2 e%9 *1 D0 *2 e%4 *1 D16 *2 e%4 *1 D0 *2 e%5 *1 *4 *2 e%9 *1 *6 *2 e%4 *1 *8 *2 e%5 *1 *6 *2
MSXD e%4 *1 *4 *2 e%9 *1 D0 *2 e%5 *1 D16 *3 e%4 *1 D0 *3 e%4 *1 *4 *3 e%9 *1 *6 *3 e%5 *1 *8 *3 e%4 *1 *6 *3
MSXD e%5 *1 *4 *3 e%9 *1 D0 *3 e%4 *1 D16 *3 e%4 *1 D0 *3 e%5 *1 *4 *3 e%9 *1 *6 *3 e%4 *1 *8 *3 e%5 *1 *6 *3
MSXD e%4 *1 *4 *3 e%5 *1 *6 o6 v12 c%4 *1 D32 o6 v12 c%5 *1 D64 o6 v12 v11 c%4 *1 D32 o6 v11 c%4 *1 *6 o6 v11
MSXD c%1 & v10 c%8 *1 D-64 *18 c%5 *1 D-96 *18 c%4 *1 D-64 *18 c%5 *1 *6 *18 c%9 *1 D32 *18 c%4 *1 D64 *18 c%4
MSXD *1 D32 *18 c%5 *1 *6 *18 c%9 *1 D-64 *18 c%4 *1 D-96 *18 c%5 *1 D-64 *23 *1 *6 o6 v9 c%9 *1 D32 *28 *1
MSXD D64 *23 *1 D32 *23 *1 *6 o6 v9 c%9 *1 D-64 *28 *1 D-96 *23 *1 D-64 *28 *1 *6 o6 v9 c%9 *1 D32 *23 *1 D64

emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/msx_vgm2mml$ head -n 100 outputs/psg_opll/vgmrips.net/FFMSX/FFMSX07.vgm/FFMSX07.mml
;[name=FFMSX07 lpf=1]
#opll_mode 0
#alloc { 0=116, 3=621, 1=12265, 2=494, 9=403, a=409, b=472, c=479, d=375, e=366 }
#psg_tune { 3421, 3228, 3047, 2876, 2715, 2562, 2419, 2283, 2155, 2034, 1920, 1812 }
#tempo 225
#title { "[MSX2] Final Fantasy(1989) Town Nobuo Uematsu, Tadahiro Nitta"}

;-----------------------  psg part -------------------------------
@e00 = { 0, 0, F }
@e01 = { 0, 0, C:2, B:2, A:2, 9:138 }
@e02 = { 0, 0, C:3, B:2, A:68 }
@e03 = { 0, 0, C:2, B:2, A:68 }
@e04 = { 0, 0, C:3, B:2, A:2, 9:2, 8:2, 7:2, 6:6 }
@e05 = { 0, 0, C:2, B:2, A:2, 9:2, 8:2, 7:2, 6:6 }
@e06 = { 0, 0, A, 9 }
@e07 = { 0, 0, B, A }
@e08 = { 0, 0, C, B }

;-----------------------  OPLL part -------------------------------
; sync marks: all active channels at loop-safe token boundaries; clock: %48 = quarter

; ch3 --- step 0 : start ---
*0 = { b64 > \-1 c32 \0 c64 \1 c64 \0 c64 }
*1 = { d+64 \7 d+64 \8 d+64 \-1 e32 \0 e64 \1 e64 \0 e64 \-1 }
*2 = { d64 \0 d64 \10 c+32 \9 c+64 \8 c+64 \9 c+64 \10 }
*3 = { \-1 c32 < \11 b64 \10 b64 \11 }
*4 = { > c%54 < g%54 e%54 d%54 g%54 b%54 d%54 c%54 a%54 e%54 c%54 e%54 g%54 > c%54 < e%54 c%54 a%54 f%54 c%54 < b%54 >
d%54 g%54 b%54 e%54 > c%54 < g%54 e%54 d%54 e%54 g+%54 b%54 @9 ) e%216 f+%108 d%108 < b%216 > c%54 < b%54 a%54 g%54
f%255 & f%177 g%255 & g%69 f%108 }
*5 = { \0 a32 \6 g+64 \5 g+64 \6 g+64 \0 a32 \1 a64 \2 a64 \1 a64 \-1 v12 g32 \7 ( f+64 \6 f+64 \7 ( f+64 \-1 g32 \0 g64
\1 g64 \0 g64 \-1 g32 \7 f+64 \6 f+64 \7 f+64 \-1 g32 \0 g64 \1 v12 d64 \0 d64 \10 ( c+32 \9 ( c+64 \8 c+64 \9 c+64 \10
c+32 }
*6 = { e32 \8 d+64 \7 d+64 \8 d+64 \-1 }
*7 = { e32 \8 d+64 \7 d+64 \8 v12 e64 \0 @e8 v15 f32 \1 @e0 v11 f64 \2 ( f64 \1 f64 \0 f32 \8 e64 \7 e64 \8 e64 \0 f32
\1 f64 \2 f64 \1 f64 \0 f32 \8 v12 d+64 \7 d+64 \8 ( d+64 \-1 @e7 v15 e32 \0 @e0 v10 e64 \1 e64 \0 e64 \-1 e32 \8 }
*8 = { g%108 d%108 e%54 f%54 e%54 d%54 c%216 c%108 a%162 a%54 g%54 d%54 e%54 f%54 e%255 & e%69 e%108 > c%162 c%54 c%108
< b%54 a%54 g%54 d%54 e%54 f%54 e%108 c%108 d%255 & d%15 < a+%54 > c%54 d%54 c%216 < b%108 }
*9 = { v13 o4 c%213 < b%216 a%216 g%216 f%216 f%216 > c%216 < e%216 a%54 > e%54 a%54 e%54 d%108 f+%108 < g%54 > d%54
g%54 d%54 c%108 < c%108 < a+%54 > f%54 a+%54 > c%54 d%216 o2 g%54 > d%54 g%54 a%54 b%54 a%54 }
*10 = { c+32 \0 d64 \1 }
*11 = { e64 \0 e64 \-1 }
*12 = { \-1 c32 < \5 b64 \4 b64 }
*13 = { c64 \0 c64 }
*14 = { b64 \10 b64 \11 }
*15 = { g+64 \6 g+64 \0 a32 \1 a64 \2 a64 \1 a64 \0 a32 \6 }
*16 = { b64 \0 b64 \-1 b32 \12 a+64 \11 a+64 \12 a+64 \-1 }
*17 = { f+64 \-1 g32 \0 g64 \1 g64 \0 g64 \-1 g32 }
*18 = { \-1 c32 < \11 b64 \10 b64 > \9 v12 c+64 \10 @e8 v15 c+32 \0 @e0 v11 d64 \1 ( }
*19 = { b64 > \-1 @e6 v15 c32 \0 @e0 v9 c64 \1 }
*20 = { v12 g+64 \5 g+64 \6 ( g+64 \0 @e7 v15 a32 \1 @e0 v10 a64 \2 a64 \1 a64 }
*21 = { \9 v12 c+64 \8 c+64 \9 ( c+64 \10 @e7 v15 c+32 \0 @e0 v10 d64 \1 }
*22 = { e32 \0 e64 \1 }
*23 = { d+64 \7 d+64 \8 ( d+64 \-1 }
*24 = { d64 \0 d64 \10 ( c+32 \9 c+64 \8 c+64 \9 c+64 \10 }
*25 = { c32 < \11 }
*26 = { ( b64 > \-1 @e7 v15 c32 \0 @e0 v10 c64 \1 }
*27 = { b64 > \-1 c32 \0 c64 }
*28 = { d64 \0 d64 \10 c+32 }
*29 = { \7 f+64 \6 f+64 \7 }
*30 = { b64 \4 b64 \5 }
*31 = { e32 \8 }
3 @0 /1 o4 \0 @e4 v15 e%57 > \-1 @e5 c%54 < \0 g%54 e%54 \-1 d%54 \0 g%54 \-1 b%54 d%54 \0 c%54 a%54 e%54 c%54 e%54
3 g%54 > \-1 c%54 < \0 e%54 c%54 a%54 f%54 c%54 < b%54 > \-1 d%54 \0 g%54 \-1 b%54 \0 e%54 > \-1 c%54 < \0 g%54 e%54
3 \-1 d%54 \0 e%54 g+%54 \-1 b%54 > @e1 e%216 \0 f+%108 \10 c+%108 < \-1 b%216 > c%54 < b%54 \0 a%54 g%54 f%255 & f%177
3 g%255 & g%69 f%108 @e5 e%36

; ch1 --- step 0 : start ---
1 @0 /1 o5 \-1 @e0 v12 e%9 \8 ( *23 *22 *11 *6 *22 ( *11 *31 *1 *6 e64 v12 g64 \0 g64 \1 ( g64 \0 g64 \-1 ( g32 *29 *17
1 *29 ( *17 *29 *17 *21 *2 *10 *24 *10 *2 *10 v12 *11 ( *31 ( *1 *6 e64 \0 v12 f64 \1 f64 \2 ( f64 \1 f64 \0 ( f32 \8
1 e64 \7 e64 \8 e64 \0 f32 \1 f64 \2 f64 \1 f64 \0 f32 \8 e64 \7 e64 \8 v12 d+64 \-1 @e8 v15 e32 \0 @e0 v11 e64 \1 (
1 *11 *31 *1 e32 *21 *2 *10 d64 \0 d64 \-1 v12 *25 ( *14 ( *0 *3 *27 \1 ( *13 *3 *0 *3 b64 > \-1 c64 ( & *13 \1 *13 *3
1 *0 *3 ) *0 \-1 *25 b64 \10 ) b64 \11 *0 \-1 *25 v12 *14 *26 *13 *3 *0 \-1 ( *25 *14 *0 *3 *27 \2 v12 a64 \1 a64 \0 (
1 a32 \6 ( g+64 \5 *15 g+64 \5 g+64 \6 g+64 \0 @e6 v15 a32 \1 @e0 v9 a64 \2 a64 \1 a64 \0 a32 \6 g+64 [\5 *15 g+64]2 \5
1 ) *15 *20 *5 \0 d64 \1 *2 c+64 \-1 v12 e64 \0 e64 \1 ( *11 ( *31 *1 *7 *1 ( *31 *1 *6 *22 ( *11 *31 *1 *6 e64 ) e64
1 \0 e64 \1 *11 e32 [\8 *1 e32]3 \8 ) *1 *31 *1 v12 *31 ( *23 *22 *11 *6 *22 ( *11 *31 *1 *6 e64 > v12 *13 \1 ( *13 \-1
1 ( c32 < \5 *30 *0 *12 \5 ( *0 [*12 \5 *0]2 \-1 c64 ) c64 < \5 *30 *0 \-1 v12 c32 < \5 ( *30 ( *0 *12 \5 *27 \1 v12
1 *13 \-1 ( c32 < \5 ( *30 *0 *12 \5 *19 *13 *12 \5 *0 *12 \6 v12 a+64 \0 @e8 v15 b32 \1 @e0 v11 b64 \2 ( b64 \1 b64 \0
1 b32 \6 a+64 \5 a+64 \6 a+64 \0 b32 \1 b64 \2 b64 \1 b64 \0 b32 \6 *20 *5 \0 d64 \1 *2 c+64 \-1 v12 e64 \0 e64 \1 (
1 *11 ( *31 *1 *7 *1 ( *31 *1 *6 *22 v12 *13 \-1 ( *25 ( *14 *0 *3 *19 *13 *3 *0 *18 *2 *10 *28 \9 ( c+64 \8 c+64 \9
1 c+64 \10 *10 *2 *10 *24 *10 *2 *10 ) *2 c+32 \0 ) d64 \1 *2 @e6 v15 c+32 \0 @e0 v9 d64 \1 *2 *10 *28 \9 c+64 \8 c+64
1 < \13 v12 a64 \0 @e8 v15 a+32 \1 @e0 v11 a+64 \2 ( a+64 \1 a+64 \0 a+32 \13 a64 \12 a64 \13 a64 \0 a+32 \1 a+64 \2
1 a+64 \1 a+64 \0 a+32 \11 v12 *14 *26 *13 *3 *0 \10 v12 c+32 \9 ( c+64 \8 c+64 \9 ( c+64 \10 *10 *2 *10 v12 *13 \-1 (
1 *25 ( *14 *0 *3 *19 *13 *3 *0 *3 ( *0 *3 *0 \-1 *25 ) *14 *0 \-1 c64 ) c64 < \11 *14 *0 < \-1 v12 b32 \12 ( a+64 \11
1 a+64 \12 ( a+64 \-1 b32 \0 b64 \1 *16 b32 \0 b64 \1 ( *16 b32 \0 b64 \1 *16 b64 > v12 *13 \1 ( *13 \-1 ( *25 *14 *0
1 *18 *2 *10 *28 \8 v12 *23 @e7 v15 e32 \0 @e0 v10 e64 \1 *11 *6 e32
; ch1 --- step 3504 : end ---

; ch2 --- step 0 : start ---
2 @0 /1 o4 \0 @e2 v15 c%219 < @e3 b%216 \-1 a%216 g%216 [f%216]2 > \0 c%216 < \-1 e%216 a%54 > \0 e%54 a%54 e%54 \-1
2 d%108 \0 f+%108 < \-1 g%54 > d%54 \0 g%54 \-1 d%54 \0 c%108 < c%108 < a+%54 > \-1 f%54 \0 a+%54 > c%54 \-1 d%216 o2
2 g%54 > \44 c+%54 \-1 g%54 a%54 \0 b%54 \-1 a%54 g%108 > \0 @e1 c16

; ch9 --- step 0 : start ---
9 @2 v13 o5 e%111 *8 @6 > c%54 d%54

; cha --- step 0 : start ---
a r4 @3 v13 o5 e%108 *8 @8 ( > c%54 d%54
; cha --- step 3504 : end ---

; chb --- step 0 : start ---

```


## Gra2 rerun update — 2026-10-20 (record only)

User reran the complete gra2_msx directory and requested recording only,
with 1% remaining. Reported 36/58 successful entries and 22/58
MUC input-capacity failures. Whole songs and sync-mark excerpts are separate
entries. Individual rejected byte sizes and new diagnostic run paths are
preserved below. No compilation-timeout, disk-full or ownership failure is
shown in this pasted rerun. Success is a reported generation result, not a
new playback-equivalence measurement. No cause of large output was inferred.
The pasted command places --force on a separate displayed line; preserve it
as reported without asserting a reconstructed invocation or executing it.

Next after reset remains PSG->MUC musical grouping/structure/envelope/macro
inspection, with the FFMSX size comparison as an important reference. No
investigation, code changes, conversion, tests, stage/commit/push this turn.

### Original Gra2 rerun report

```text
Gra２も再実行しました。
やはりサイズが大きい。枠は残り1%なので記録だけしてください

emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/psg_scc --outdir outputs/psg_scc/ --target mucomsx --force
psg_scc/      psg_scc_opll/
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/psg_scc/msxplay.com/ --outdir outputs/psg_scc/ --target mucomsx --force
COPYRIGHT.md  gra2_msx/     p52_arbl/
emef220@Legion570i:/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml$ python scripts/export_mucom.py ../msx_vgm2mml/tests/fixtures/local_only/psg_scc/msxplay.com/gra2_msx/ --outdir outputs/psg_scc/msxplay.com/gra2_msx/ --target mucomsx
--force
gra2_001/gra2_001.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 266198/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_001/run__o31bh5f/report.txt
gra2_001/with_sync_mark/gra2_001_with_sync_mark01.vgm: success
gra2_001/with_sync_mark/gra2_001_with_sync_mark02.vgm: success
gra2_001/with_sync_mark/gra2_001_with_sync_mark03.vgm: success
gra2_001/with_sync_mark/gra2_001_with_sync_mark04.vgm: success
gra2_001/with_sync_mark/gra2_001_with_sync_mark05.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 65953/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_001_with_sync_mark05/run_cwkirmi1/report.txt
gra2_001/with_sync_mark/gra2_001_with_sync_mark06.vgm: success
gra2_002/gra2_002.vgm: success
gra2_003/gra2_003.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 238119/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_003/run_kilxtyxk/report.txt
gra2_003/with_sync_mark/gra2_003_with_sync_mark01.vgm: success
gra2_003/with_sync_mark/gra2_003_with_sync_mark02.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 92764/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_003_with_sync_mark02/run_i8_061_i/report.txt
gra2_004/gra2_004.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 268955/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_004/run_3qboweus/report.txt
gra2_004/with_sync_mark/gra2_004_with_sync_mark01.vgm: success
gra2_004/with_sync_mark/gra2_004_with_sync_mark02.vgm: success
gra2_004/with_sync_mark/gra2_004_with_sync_mark03.vgm: success
gra2_005/gra2_005.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 68799/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_005/run_63vrk0_m/report.txt
gra2_005/with_sync_mark/gra2_005_with_sync_mark01.vgm: success
gra2_006/gra2_006.vgm: success
gra2_006/with_sync_mark/gra2_006_with_sync_mark01.vgm: success
gra2_007/gra2_007.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 146932/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_007/run_59j1isjn/report.txt
gra2_007/with_sync_mark/gra2_007_with_sync_mark01.vgm: success
gra2_007/with_sync_mark/gra2_007_with_sync_mark02.vgm: success
gra2_007/with_sync_mark/gra2_007_with_sync_mark03.vgm: success
gra2_008/gra2_008.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 76003/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_008/run_ecmars7d/report.txt
gra2_008/with_sync_mark/gra2_008_with_sync_mark01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 75566/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_008_with_sync_mark01/run_8fgnamtr/report.txt
gra2_009/gra2_009.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 140296/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_009/run_ej5jzdmh/report.txt
gra2_009/with_sync_mark/gra2_009_with_sync_mark01.vgm: success
gra2_009/with_sync_mark/gra2_009_with_sync_mark02.vgm: success
gra2_010/gra2_010.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 301790/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_010/run_dtbrfp3k/report.txt
gra2_010/with_sync_mark/gra2_010_with_sync_mark01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 87958/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_010_with_sync_mark01/run_yjdj67pk/report.txt
gra2_010/with_sync_mark/gra2_010_with_sync_mark02.vgm: success
gra2_011/gra2_011.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 188411/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_011/run_uuclhx1h/report.txt
gra2_011/with_sync_mark/gra2_011_with_sync_mark01.vgm: success
gra2_011/with_sync_mark/gra2_011_with_sync_mark02.vgm: success
gra2_011/with_sync_mark/gra2_011_with_sync_mark03.vgm: success
gra2_012/gra2_012.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 109662/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_012/run_ibzrutwt/report.txt
gra2_012/with_sync_mark/gra2_012_with_sync_mark01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 109671/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_012_with_sync_mark01/run_rq21o4bk/report.txt
gra2_013/gra2_013.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 114225/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_013/run_mt0q_w_j/report.txt
gra2_013/with_sync_mark/gra2_013_with_sync_mark01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 114243/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_013_with_sync_mark01/run_myqbsn07/report.txt
gra2_014/gra2_014.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 271416/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_014/run_9qdj1r44/report.txt
gra2_014/with_sync_mark/gra2_014_with_sync_mark01.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark02.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark03.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark04.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark05.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark06.vgm: success
gra2_014/with_sync_mark/gra2_014_with_sync_mark07.vgm: success
gra2_015/gra2_015.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 170085/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_015/run_mmca2q8w/report.txt
gra2_015/with_sync_mark/gra2_015_with_sync_mark01.vgm: success
gra2_015/with_sync_mark/gra2_015_with_sync_mark02.vgm: success
gra2_015/with_sync_mark/gra2_015_with_sync_mark03.vgm: success
gra2_015/with_sync_mark/gra2_015_with_sync_mark04.vgm: success
gra2_016/gra2_016.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 290966/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_016/run__8d6_fgq/report.txt
gra2_016/with_sync_mark/gra2_016_with_sync_mark01.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 82491/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_016_with_sync_mark01/run_0cww482w/report.txt
gra2_016/with_sync_mark/gra2_016_with_sync_mark02.vgm: success
gra2_016/with_sync_mark/gra2_016_with_sync_mark03.vgm: conversion_failed: MUC conversion failed: MUCOMSX MUC input capacity exceeded: 135737/65536 bytes; report=/mnt/i/wsl/repositories/emef2247/test/mucom88_vgm2mml/outputs/psg_scc/msxplay.com/gra2_msx/_diagnostics/gra2_016_with_sync_mark03/run_nawv5j2j/report.txt
gra2_017/gra2_017.vgm: success
gra2_017/with_sync_mark/gra2_017_with_sync_mark01.vgm: success

```
