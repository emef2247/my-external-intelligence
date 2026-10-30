# MUCOMSX environment and SCC transfer checkpoint

User suspended scc_vgm_enhancer and prioritized MUCOMSX. Confirmed explicit
policy: consider all five SCC parts for FM enhancement; old provisional four-part
CLI default is not accepted. Enhancer final review found unresolved EOF FM stop,
same-sample loop-anchor and stale-output issues. Do not port that emitter.

Installed official v1.0-stable MucoMSX_261016.zip, SHA256
582a72344d30982f79b61135f82746813c8b739ad7a4c00c7cb5530d3f4a42d6,
in ignored mucom88/.tools/mucomsx-v1.0-stable with full notices/licenses. New setup
and staging tools preserve edited data, source-owned implicit banks and fresh
artifact lifecycle. Eight public safety checks passed.

User authorized boot media:
C:/Users/ef110/Downloads/ESERAMAir_MUCOM.dsk
SHA256 1e30f8125d2bf7cd5caff7d392d6e15fa363eb1e785ae0654da3636795c494f3.
This contains COMMAND2.COM + NEXTOR.SYS; original unchanged. A working copy was
used with headless openMSX, existing FS-A1GT ROMs, 512KiB mapper, SunriseIDE Nextor
2.1.0 ROM-only extension. Do not publish disk/ROM/DOS or backup contents.

Additional user-provided private alternatives (not accessed for this milestone):
H:/_env/MSX/disk_images/MSX0/SD4G_Backup/DSK/MSX_DOS2_ENG.DSK
H:/_env/MSX/disk_images/MSX0/SD4G_Backup/DSK/MSX_DOS2_JPG.DSK
H:/_env/MSX/TNI/CMD2-244 (COMMAND version 2.44)
H:/_env/MSX/MFR_Data_Backup
User explicitly says none of these belongs on GitHub.

One authored ordinary SSG MUC was executed; no private music. MUC2MUB and
MUCPLAY session MUB outputs were both302bytes and byte-identical, MUB8, SHA256
3d0fd85fe7035ead23a14fb49d6d143a39d45bdf06620420232eeade7417a8e1.
VGM export finished:1145bytes,v1.51,220928samples,200OPNAwrites, declared8MHz.
This differs from old7987200Hz target/probe assumptions; validate explicit pitch
and timer conventions in the MUCOMSX adapter, not a silent shared change.
SCC3/FM3/MSX-PSG source patterns are staged only. Initial90virtualseconds did not
finish session export; bounded capture showed progress (61emu sec/20wallsec),
not a documented interactive wait. A100wallsec window completed all commands.
No need further environment debugging. Artifacts: outputs/mucomsx_smoke.

Official limits and three-SCC-part constraint are documented in public English
docs/mucomsx_setup.md and profile JSON. This milestone does not connect native
SCC5 or CH3 VGM-source interpretation. Next use explicit MUCOMSX profile/container
adapter, retain MSX musical units/volume runs and source IR, then independent
OPNA fit/MUC projection; no wholesale register-emitter copy or raw-VGM bypass.
Existing dirty changes preserved; no stage/commit/push in either repository.
