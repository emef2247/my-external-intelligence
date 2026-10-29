# Multi-bank PDX reference index from the saved census — 2026-10-30

User recalled the earlier multi-bank report and requested durable references.
The results WERE already recorded in2026-10-27_pdx_library_and_mkpdx68.md.
This note adds an explicit file index so future work does not repeat the scan.
Re-read saved CSV metadata only: no H: rescan, conversion or playback.

## Library census

Root: H:/_env/D/MDX, scanned2026-10-27 (not all of H:/_env/D).
853 extension-matching files;844 had validated expanded table/range layouts.
817 had one populated logical96-slot bank;27 had multiple populated banks.
The maximum was16 populated banks in17 table groups (bank0 empty).
Ten compressed files were expanded in memory for the census. File bytes and
expanded PDX bytes are different metrics. Table capacity was inferred from
layout; PDX has no explicit bank-count field. Populated bank count is NOT proof
that a corresponding MDX references each bank: no complete MDX-use census was
performed for this directory.

Durable original scan metadata remains in this converter's ignored directory:
outputs/diagnostics/pdx_library_20261027/{analysis_ja.md,summary.json,
pdx_files.csv,pdx_banks.csv,pdx_slots.csv,audit.py}.
The user backs up outputs separately. pdx_banks.csv retains per-bank slot count,
logical sample-byte sum and physical referenced-range union; pdx_files.csv
retains whole expanded size, table prefix, gaps/tail, aliases, duplicates and
hashes. Do not call a logical payload sum an independently loadable bank size.

| Path relative to library root | Expanded PDX bytes | Inferred96-slot capacity groups | Populated banks | Live slots |
|---|---:|---:|---:|---:|
| Capcom/Street_Fighter/SSF2/CAM2.PDX | 400000 | 3 | 3 | 40 |
| Konami/Gradius/Grad2/CHAMA/GRA2C.PDX | 306970 | 2 | 2 | 56 |
| Konami/Other/bell_20_.PDX | 167581 | 4 | 3 | 30 |
| Konami/Other/WW01.PDX | 205072 | 3 | 3 | 37 |
| Konami/Popn_Twinbee/POPN.PDX | 299606 | 4 | 3 | 43 |
| Konami/Popn_Twinbee/popn_01_.PDX | 524266 | 5 | 5 | 59 |
| Konami/Space_Manbow/GRADIUS.PDX | 421896 | 5 | 4 | 58 |
| Konami/TMNT/YURAYSAN/TMNT.PDX | 414321 | 5 | 5 | 88 |
| Konami/Twinbee_Detana/KOU/TWINBEE_02.PDX | 1257158 | 6 | 6 | 157 |
| Namco/Blue_Crystal_Rod/BLUECRY.PDX | 248087 | 2 | 2 | 50 |
| Namco/Galaga_88/G8MUS.PDX | 67706 | 2 | 2 | 14 |
| Namco/Other/runupc.pdx | 311040 | 3 | 3 | 66 |
| Namco/Xevious/S_XEVIK.PDX | 113413 | 3 | 2 | 12 |
| Nintendo/Mario_Paint/MPAINT.PDX | 768557 | 17 | 16 | 197 |
| Other/Misc/UMI.pdx | 220762 | 3 | 3 | 28 |
| Other/Sampler/rr4_your.pdx | 322900 | 9 | 7 | 55 |
| Other/Sampler/sm64_st1.pdx | 191444 | 2 | 2 | 27 |
| Other/Sampler/smet_br2.pdx | 129525 | 5 | 5 | 26 |
| Other/Sampler/tobd_pob.pdx | 124456 | 10 | 7 | 30 |
| Other/Sampler/wr64_twi.pdx | 165994 | 6 | 3 | 35 |
| SEGA/Sonic_CD/SONIC04.PDX | 147397 | 4 | 4 | 45 |
| SEGA/Turbo_Outrun/TO92.PDX | 359028 | 3 | 3 | 35 |
| SNK/Fatal_Fury/gold03.pdx | 467477 | 4 | 4 | 80 |
| SNK/Psycho_Soldier/Psycho_AR.PDX | 352845 | 5 | 5 | 43 |
| SNK/View_Point/NAT.pdx | 61469 | 2 | 2 | 11 |
| Taito/Other/lb06c.pdx | 310721 | 3 | 3 | 61 |
| Taito/RayForce/CHAMA/RAY01C.PDX | 840134 | 3 | 3 | 156 |

## Stronger paired MDX/PDX reference: Fujix Armed Police Batrider

Separate source root (not assumed present in the H: census):
tests/fixtures/local_only/from_Fujix/Raizing/ArmedPoliceBatride/.
BR_ADP.PDX:718130bytes total, four96-entry table groups (3072table bytes),
bank0 empty; banks1/2/3 have47/32/35 populated slots,114 distinct payloads total.
The26paired MDX command walks actually referenced banks1,2,3, with no missing
populated sample references. Static usage, not native playback proof.
See2026-10-21_fujix_reference_banks_and_footprint.md and ignored
outputs/diagnostics/fujix_reference_20261021/{summary.json,per-PDX slot CSVs,
per-MDX PCM-track CSVs}. BattleGaregga/GR_PCM.PDX is the paired single-bank
comparison:475413bytes,74 live slots, bank0 only.

## What these references establish and do not establish

The examined MXDRV2.06+17Rel.X5-S PCM FD command selects a bank; sample lookup
uses (bank*96+note)*8 relative to the whole loaded PDX. Therefore96slots is
per bank in this implementation. It is not evidence of on-demand loading or
independent312KiB allocations per bank. More banks alone do not reduce the
resident whole-file memory requirement. A larger PDX can require a larger
resident buffer even if each logical bank is small.

Do not infer multi-bank support from the bundled mdxtools playback path: its
PDX loader hardcodes96entries and PCM FD is ignored in that runtime. Future
converter work needs explicit target binding(bank,slot), compiler/assembler
bank emission, final packer table validation, supported driver playback, and
separate whole-file/bank-local accounting. Do not merely raise SLOT_LIMIT.
Current single-bank converter and conservative315122-byte default cap are
unchanged by this reference-index task. No source payloads copied into notes.


## Files exceeding the initial312KiB buffer (follow-up,2026-10-30)

Recounted the saved validated_ranges CSV rows, not a fresh H: scan:
20/844 expanded PDX files exceed319488bytes, all20distinct content hashes.
Nine have only one populated bank;11have multiple populated banks. Therefore
larger resident allocations are relevant even without multi-bank support.
Only one exceeds1200000decimal bytes (also exceeds1200KiB):
Konami/Twinbee_Detana/KOU/TWINBEE_02.PDX,1257158bytes,6populated banks/157slots.
Other examples: Taito/RayForce/CHAMA/RAY01C.PDX840134bytes/3banks;
Nintendo/Mario_Paint/MPAINT.PDX768557bytes/16populated banks;
Other/Misc/VATLVA10.pdx654383bytes/1bank;
Other/Battle_Garegga/GR_PCM.PDX475413bytes/1bank.
The separately supplied Fujix BR_ADP.PDX is718130bytes and GR_PCM.PDX475413,
both above312KiB. Their paired static MDX reference checks were recorded on
2026-10-21; do not claim current native playback was repeated.

The examined MXDRV2.06+17Rel.X5-S -p:<num> option sets the resident buffer in
KiB;312KiB is an initial allocation, not a format maximum. This was already
verified from source in the2026-10-27 census. The supplied old environment
used MADRV; its installed/current options must not be inferred from MXDRV's.
File existence and validated table ranges do not prove each file plays on the
user's installed resident configuration. Total RAM, resident allocation and
loader's simultaneous temporary copy are separate. A1.2MB-class historical
PDX is consistent with the user's floppy-sized-data account, but the exact
floppy format/available payload was not measured here. Decimal1200000bytes
and binary1.2MiB are different; retain exact bytes in tests.

No converter limit or guest/driver configuration changed during this follow-up.
The315122-byte converter default is still the conservative test policy from
the preceding task, not a newly asserted universal PDX limit.
