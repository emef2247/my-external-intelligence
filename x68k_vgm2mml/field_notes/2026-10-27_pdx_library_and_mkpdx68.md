# PDX library census and mkpdx68 review (2026-10-27)

User requested read-only H:/_env/D/MDX census to identify constraints behind
MMDSP PCM-buffer failures, then explicitly added YosAwed/mkpdx68 research.
No converter defaults/limits/algorithms changed, no private conversion or
listening, no changes to H: files. User is running MSX run_test.sh separately;
do not rerun or modify that checkout until their completion notice.

Full ignored results: outputs/diagnostics/pdx_library_20261027/analysis_ja.md,
audit.py, summary.json, pdx_files.csv, pdx_banks.csv, pdx_slots.csv. No audio
payloads were copied from the library. Isolated unmodified reference decoder
expanded ten LZX042 PDX files in memory; final native table/ranges validated.
Public external code remains ignored, not incorporated into production.

853 PDX-extension files, disk bytes78830926. Validated844 including10compressed;
empty1 and8 unrecognized/variant/invalid files are excluded from table metrics,
not declared universally corrupt. 844 files have760 distinct expanded hashes;
84 are duplicate file contents. Two generated vgm2mml files included; removing
them does not materially change the median. No complete MDX command/reference
census or peak-polyphony measurement was performed.

Expanded PDX file median49750B,p95319423B,max1257158B. Nonempty logical96-slot
bank physical-reference union median47615B,p95318149B,max653615B. 14998 live
slots, median3784B,p9514000B,max450576B. Nine lengths>65535 in8files; PDX alone
does not specify ADPCM vs linear PCM. 817/844 have one populated bank,27multiple;
max16populated banks in17 table groups with bank0empty. 26/64slot compact and
128slot/padded prefixes occur: inferred capacity is not actual MDX bank usage.
30files alias same physical ranges;26duplicate content at separate addresses.
Header count is inferred, not an explicit PDX field. All byte budgets count
whole expanded file including tables/gaps/tail, not logical sample-size sum.

Important newly verified source fact: MXDRV2.06+17Rel.X5-S disassembly Start
sets pdx_size=0x4e000=319488B (312KiB), code line2528; HelpString2718 onward
also says312. -p:<num> sets KiB, via shift10 in L001966. This supersedes earlier
statements that this particular reference's default was unmeasured. It DOES
NOT identify user-installed resident driver/version/options or MADRV default.
MMDSP MXCTRL TRANS_PDX adds270B wrapper; conditional usable file319218B:
792/844 (735/760distinct) pass capacity comparison, versus824/844 raw-only.
Do not conflate this with the NanoDrive8 user policy1200000B.
L_SETPDX696-737 admits equality in comparison, but DBRA copy loop appears to
copy4additional bytes; boundary safety unverified, no exact-capacity guarantee.
Temporary file buffer remains allocated until after transfer, so resident
request size is not total peak RAM. No NanoDrive8 decoded5MB accounting imposed.

mkpdx68 README blob68cf635766514ca4dee4bd5ea81101782ab49e58, code blob
 eb0f4ace77da51bf753475fe62471c6ce33f763d. Single96-slot bank, BE32offset/size,
raw ADPCM concatenation, no transcode/resample/dedup/runtime-budget detection.
Despite standard label, no65535length check: authored65536B packs with u32len.
@0accepted,@1rejected. Empty slots have nonzero current offsets and size0;
this is legitimate. Only totalu32 bound, not our1200000B or PCM8u24 enforcement.
Changing packers alone won't solve resident memory shortage. Built unchanged
source only in ignored diagnostic setup;6 authored cases verified byte payload,
lengths32/65535/65536,duplicate allocation,@0/@1; no native playback claim.
Parser6 authored selfchecks also passed. Focused architect review reconciled
census and caught wrapper-count and temporary-memory/boundary caveats, now saved.

Recommended later implementation (not started): configurable resident-byte
budget plus wrapper allowance, propagated to sample representative reduction
and final packed check, separately from NanoDrive8 format/operational caps.
Banks do not provide on-demand loading;64KB sample/96 total slots aren't
universal format constraints. Do not pick corpus percentile as universal limit.
Current user playback environment and loader selection must calibrate a profile.

Primary URLs:
https://github.com/gaolay/MMDSP/blob/master/src/MXCTRL.s
https://github.com/gaolay/MMDSP/blob/master/src/MACTRL.s
https://github.com/vampirefrog/x68kd11s/blob/master/sound/mxdrv/2.06%2B17_Rel.X5-S/mxdrv17.s
https://github.com/YosAwed/mkpdx68/blob/main/mkpdx68.cpp
https://github.com/Fujix1/lzx/blob/main/src/lzx042.js
No stage/commit/push. Preserve pre-existing MSM6295 working-tree work.
