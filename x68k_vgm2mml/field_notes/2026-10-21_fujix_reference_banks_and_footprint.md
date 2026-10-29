# Fujix reference structure and target footprint (2026-10-21)

## Scope and evidence

User supplied private `tests/fixtures/local_only/from_Fujix` for analysis, with
screenshots reporting PDX memory limits, streaming behavior and multibank use.
Read-only encoded-data analysis only: no private conversion, listening, replay,
native decoder simulation or regression. Source files were not modified.
Inspect ignored `outputs/diagnostics/fujix_reference_20261021/`: `audit.py`,
`summary.json`, per-PDX slot CSV and per-MDX PCM-track CSV. Hashes in the JSON
identify the analyzed versions. Copyrighted payloads are not reproduced here.

## Measured MDX/PDX

| Set | MDX count | MDX byte range | PDX bytes incl. table | Tables / populated slots | Banks referenced |
|---|---:|---:|---:|---|---|
| ArmedPoliceBatride | 26 | 155–16,613 | 718,130 | 4 x 96 / 114 | 1, 2, 3 |
| BattleGaregga | 15 | 1,228–13,868 | 475,413 | 1 x 96 / 74 | 0 |

BR_ADP.PDX has 3,072 table bytes; bank 0 is empty and banks 1/2/3 contain
47/32/35 nonempty entries. All 114 exact payload hashes are distinct. Lengths
are 624–38,677 bytes, median 5,321. Last referenced payload ends at 709,592,
leaving 8,538 trailing file bytes; do not call every byte after the table used
sample data. The entire file, including this tail, counts toward the budget.
GR_PCM.PDX has 768 table bytes, 74 distinct nonempty payload hashes and lengths
780–52,748 bytes, median 3,514. Referenced data ends at EOF.

All 41 MDX files have regular 16-track tables, initial track offset 34. PCM
rate operands encountered are 4. At most 4/2 PCM tracks respectively contain
encoded note commands; this is NOT measured peak simultaneous polyphony.
Static command walks encountered no unknown commands or missing populated
sample references. Finite repeats/song loops were not expanded; note-command
counts are not acoustic attacks. Loop-carried bank state and runtime/native
playback equivalence were not verified. All MDX fit both 65,535-byte current
compiler scope and the user's 256 KiB player allocation.

## Bank semantics and tool limitation

Original MXDRV 2.06+17 Rel.X5-S disassembly:
https://github.com/vampirefrog/x68kd11s/blob/master/sound/mxdrv/2.06%2B17_Rel.X5-S/mxdrv17.s

`SetVoiceCommandFunc` (FD) stores a PCM track bank selector. The PCM8 note path
`L000f28` calculates `(bank * 96 + note) * 8`, reads offset/32-bit length, and
addresses the payload relative to the whole PDX base. Thus 96 is per bank in
this implementation, not a universal whole-PDX limit. The supplied files
actually exercise nonzero banks. This does not certify all MXDRV variants or
players. The bundled mdxtools PDX loader hardcodes 96 entries and its runtime
ignores PCM FD; it cannot independently certify multibank playback. Its decode
allocation is also not a NanoDrive8 hardware specification.

Current converter deliberately emits one bank and uses a 96-slot target budget.
Changing it to multibank needs explicit compiler/assembler, binding, command,
verification and runtime support; do not just raise the slot constant.

## VGM streams (encoded command observations)

All five files with ADPCM use stream id 0, type-04 data banks, setup 90/91,
frequency 92, and block-start 95. They have no direct B7 register-1 data writes.
The setup targets MSM6258 chip id 17, register 1. Byte supply rate is 7,812 or
7,813 bytes/s (not the decoded sample rate). MSLUG Neo-Geo Logo is FM only.

| Input | Data block byte lengths | Blocks actually started |
|---|---|---|
| MSLUG Stage 1 | 169394 + 864963 | 0, 1 |
| MSLUG Stage 3 | 138683 + 951642 | 0, 1 |
| MSLUG Stage 6 | 375616 + 686263 | 0, 1 |
| MSLUG Final Scene 1P | 1131465 | 0 |
| wapico | 1033545 + 1033545 | 1 only |

MSLUG stored data totals 1,034,357 / 1,090,325 / 1,061,879 / 1,131,465
bytes respectively. wapico contains two distinct blocks but only block 1 is
started; the entire 2,075,788-byte VGM is not its played ADPCM size. Its selected
block has 1,033,545 bytes. All these selected encoded payload totals are below
1,200,000, before target packaging/encoding changes. This is not proof a current
conversion can produce equivalent playable PDX or satisfy its per-sample limit.

The streams are useful references for long-form ADPCM supply, not evidence
that original individual instruments can be recovered. Block boundaries are
transport boundaries, not independently reset instruments. Preserve predictor,
step and nibble continuity when designing future slicing; arbitrary division
and restarting each block would change audio. No source demixing claim.

## User target footprint and remaining implementation gap

User clarified NanoDrive8's layout: MDX allocation 256 KB, PDX allocation
1.25 MB, decoded PCM workspace 5 MB. User requests about 1.2 MB output ceiling.
Use a conservative operational PDX file budget of **1,200,000 bytes including
all reference tables, padding and payload**. This chosen decimal threshold is
not a measured universal hardware limit. The 5 MB decoded workspace is internal
to NanoDrive8 and excluded from converter accounting. Do not multiply PDX by
four to impose a new converter limit. MDX player allocation is separately
recorded as 256 KiB (262144 bytes); compiler/offset restrictions remain separate.
Both supplied PDX files fit. Slot count alone cannot establish fit: 96 entries
of 65535 bytes exceed six MB. Current production code has no aggregate PDX byte
budget; 96 slots and per-sample 65535 checks are not an enforcement of 1.2 MB.
This task records the requirement; no capacity algorithm was changed.

The next capacity change must budget final packed PDX, keep native source IR,
and inspect/report retained/substituted/omitted bytes and playbacks. A multibank
extension must still honor the aggregate ceiling. Do not silently equate MDX
256 KiB allocation with compilability or native playback guarantee.

## User reports not reproduced

Screenshots report software streaming volume instability, native-chip success,
AfterBurner II PCM8 CPU pressure and Super Monaco GP MXDRV16 irregular layout.
Those claims remain attributed observations. AfterBurner II / Super Monaco GP
files are absent from this supplied directory; their layout/cause is unverified.

## Compiler default request

Per user request, `export_mdx.run_batch`, export CLI and OPM roundtrip CLI now
default to mmlx. Explicit `--compiler mxc` remains. Typed PCM FM already used
mmlx. Updated README/setup/script docs and existing routing checks. 26 export
and 8 roundtrip tests passed; one public `3ch_test.vgm` export with compiler
unspecified and `--no-vgm` succeeded, results.csv records mmlx. No private
conversion/listening. An old unsupported-clock fixture now uses the unsupported
YM2164 variant because alternate positive YM2151 clocks are already supported.
The default change does not establish a cause or fix for Super Monaco GP.
