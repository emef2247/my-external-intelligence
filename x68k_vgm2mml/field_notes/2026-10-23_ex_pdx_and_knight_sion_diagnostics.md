# EX-PDX default and existing KNIGHT / Sion II diagnostics (2026-10-23)

## User scope

Investigate saved KNIGHT compilation failures and Sion II PCM blocks; make
EX-PDX the default and standard PDX an explicit option. User confirmed their
XM6 TypeG setup supports EX-PDX. User owns local_only regression/listening.
No private VGM was reconverted, rendered, tested or copied by this task.

## Existing failure evidence (read-only)

KNIGHT results.csv identifies mmlx. Every reported compilation failure is
an MDX track-start offset exceeding 0xfffe, with no compiled MDX:

| Input | Reported track | Generated MML bytes |
| --- | ---: | ---: |
| KNIGHT03 | 4 | 393843 |
| KNIGHT10 | 3 | 801608 |
| KNIGHT11 | 6 | 201913 |
| KNIGHT13 | 8 | 142174 |
| KNIGHT14 | 7 | 183247 |
| KNIGHT16 | 4 | 259347 |

The MML byte counts are not compiled MDX sizes. Artifacts are below
outputs/opm_msm6258/vgmrips.net/KNIGHT/_diagnostics/<stem>/tracks/<stem>.vgm/;
_errors/<stem>.vgm.log repeats the offset error. KNIGHT03's conversion metadata
explicitly says PCM projection is inapplicable (OPM only). This rules out
PDX sample length as its cause. A player's 256 KiB MDX allocation does not
relax the format/compiler's 16-bit track-pointer constraint. EX-PDX changes
sample length handling, not MDX layout.

Only SIONII01 and SIONII02 diagnostics survived under
outputs/opm_msm6258/vgmrips.net/Sion_II/. The saved results.csv contains only
01; user reported all eight blocked, but 03-08 cannot be attributed from the
available files. Both retained analyses report two data_outside_known_playback
writes at time zero (01 IDs262/263, 02 IDs278/279) and reserved_pan_bits
(01:0x88, 02:0x80). msm6258.py labels these source issues; pcm_mdx.project
blocks analyses with issues before compilation, including --force. They are
current source interpretation/preflight limits, not evidence of a malformed
command line or EX-PDX length overflow. No source semantics or force omission
policy was changed to bypass them in this task.

## Confirmed format / implementation evidence

soundlog0.15.0 PdxBuilder writes offset and size with BE u32; the external
Rust helper already accepts 1..0x00ffffff bytes. There is no separate EX flag.
When size<=65535 the upper size word is zero, byte-compatible with standard
PDX's reserved word plus u16 length. Above this threshold the extended reader
must consume the long length. This does not imply PCM1 compatibility.

Cached public source:
outputs/diagnostics/fujix_reference_20261021/reference_mxdrv17.s
L000f28 / around1593 reads move.l ($0004,a0),d3. L000f8e / around1632 masks
length with #$00ffffff before the PCM8 trap. This bounds this target's PCM8
call even though the EX table stores32bits. E8 selects the16track layout;
it is not a separate EX-PDX file-format flag.

Primary source URLs:
https://github.com/vampirefrog/x68kd11s/blob/master/sound/mxdrv/2.06%2B17_Rel.X5-S/mxdrv17.s
https://docs.rs/crate/soundlog/0.15.0/source/src/mdx/pdx.rs

## Implemented target behavior

--pdx-format ex-pdx is default for canonical conversion, export and verification
CLI. --pdx-format standard selects the65535-byte sample limit. EX uses the
24-bit PCM8 call limit; both retain one96slot bank, table768bytes and total
file budget1200000bytes. Source State/Segments/PCM IR remain unchanged.

Format reaches native MSM6258 preparation, reduction/capacity/parallel lanes,
Sega finite/parts bank extraction, Delta-T bank extraction, normalization
candidate/baseline/final projection and final external pack verification.
Existing mix chunk sizes, codecs, internal-loop support and metadata-memory
budgets are unchanged. No arbitrary sample splitting or decoder restart added.

Representative candidates and onset repair must fit BOTH selected sample
length and total available payload(1200000-768). No feasible representative
is a clear capacity failure; native preparation can fall through to existing
force whole-sample omission. No zero-byte/truncated substitute is fabricated.

Selected format appears in results.csv, bank console/statistics,
<stem>.pcm.timing.json and <stem>.pcm/format.json. Standard overflow is named
sample_length_overflow; total file pressure remains pdx_byte_capacity.
The helper's typed target.tsv schema is unchanged. Related current English
documents were reconciled; historical PCM1 evidence and MDX limits retained.

## Public/authored verification

185 distinct focused checks passed across PCM projection, reduction, source
bank preparation, capacity, Sega/Delta bank handling, export/report and EX
boundaries. The old65536-byte rejection tests now explicitly request standard;
they still verify rejection/force omission rather than silently losing coverage.

Public opm_segapcm:4/4 EX default and4/4 explicitstandard successful with
mmlx/typedPCM. Their small-sample PDX bytes are identical between formats.
Public native stream95_finite:1/1; its short PCM gate is intentionally omitted
by existing8ms normalization, so this is not an audible PCM validation.

Authored tests pack65535/65536/249856-byte samples and check exact table lengths
and canonical payload bytes, source unchanged, large-asset force/reuses and
parallel allocation, total-file pressure and no-feasible-candidate behavior.
Persisted authored249856B sample:PDX250624B, MDX152B, one slot, offset768,
exact payload. First ad-hoc FM-rest assembly lacked the required sharedtempo
and endtick; corrected it to @t255 and exact1772ticks, using<=256tick rests.
This was authored test setup, not a production conversion failure.

Inspectable artifacts:
outputs/diagnostics/ex_pdx_20261023/inspection.json
outputs/diagnostics/ex_pdx_20261023/authored_long/inspection.json
outputs/diagnostics/ex_pdx_20261023/authored_long/EXLONG.pcm_*.csv
outputs/diagnostics/ex_pdx_20261023/authored_long/EXLONG.pcm/format.json
No native playback, listening or compatibility certification was performed.

## Remaining work / boundaries

EX-PDX does not reduce whole-PDX load RAM or remove MDX16bit offset limits.
Sion source issues need separate semantics work if requested. Existing bank
resource budget, audible missing-ROM and internal-loop blockers remain; do not
claim AFBII01/GFII04 solved. Compact event-run voice capture was discussed in
previous context but is NOT present in this working tree; PartCollector still
has its aggregate16M frame guard. Preserve saved missing-ROM audibility
classification. Do not restart unrelated musical/enhancer work automatically.

No stage, commit or push requested/performed. External notes are a separate
private repository and not included in the public working tree.

## Follow-up: why loops did not prevent MDX overflow

Read-only static inspection; no reconversion, tests or behavior edits.
User asks whether insufficient structuring causes overflow. Saved KNIGHT03
metadata says structured notation, normalization enabled and song loop disabled
by --no-song-loop. This disables the whole-song L return, not finite repeats.

Counts of written MML syntax (not expanded runtime operations, musical gates,
or compiled command byte counts):

| Input | finite `[` | `&` | D setters | raw y writes | @v setters |
| --- | ---: | ---: | ---: | ---: | ---: |
| KNIGHT03 | 353 | 24697 | 30944 | 5442 | 2158 |
| KNIGHT10 | 1290 | 20134 | 25128 | 46698 | 4298 |
| KNIGHT13 | 294 | 9139 | 11309 | 3774 | 363 |

These are already after structure/compaction. Thus loops are present, but the
remaining representation contains many timed held-note/control fragments.
KNIGHT03 source report says204.543333s and85461 raw OPM register writes;
these source counts are not target byte counts. MML393843B likewise is not
compiled MDX393843B. Compilation failed before a current full MDX was emitted.

opm_mdx_music.build_music groups source Key gates, then emits held note slices
for timed pitch/volume/operator updates; uniform carrier TL becomes @v while
other operator changes remain y. Inner slice and outer musical-unit keys feed
shared musical_structure.source_plan. Keys retain duration and complete OPM
state/write trajectory. source_loop_plan.render accepts folding only when
expanded target tokens are equal. Therefore similar melodies with differing
fine pitch, control values, duration or onset setup do not necessarily fold.
The output is not simply one note per raw Segment, nor is every similarly
sounding phrase guaranteed to share bytes.

Current folding handles adjacent exact repeated bodies; arbitrary distant
phrase sharing/subroutine output is not implemented. Source MML macro
preprocessing alone would not reduce MDX if the compiler expands it.

Further optimization could be possible, but identifying the winning fix
requires separate target-size attribution / repeated-phrase diagnostics.
Do not call all D setters redundant just because counts are high: MDX setters
may be necessary across state changes/loop entries. Check canonical target
commands and preserved source trajectories before state canonicalization,
control coalescing, modulation inference or lossy approximation. No proof yet
that any proposed optimization makes all six failures fit.

## Correction after reviewing normalization decisions

User recalled default8ms suppression and requested both projects' FieldNotes.
Reviewed x68k2026-10-11_short_gate_checkpoint.md and
2026-10-11_clock_option_and_listening.md, plus msx2026-10-01_sample_note_length_experiment.md,
2026-10-02_note_normalization_benchmark.md and2026-10-05_mdx_pre_macro_compaction.md.

The earlier size explanation omitted that normalization already removes a
large amount of temporal detail. It must not imply all raw fine intervals
survive, or use remaining setter counts as proof of an unnormalized output.

Saved KNIGHT03 normalization confirms applied/enabled, normalization_ms8,
bound352samples, collapsed_positive_intervals64710, coalesced_rest_count7546,
and original whole-gate omissions0. Collapsed interval count is not a count
of deleted register writes. Remaining control necessity / avoidable overhead
has not been attributed by compiled-byte census.

The x68k recorded8ms policy covers original complete Key-On/Off gates plus
bounded target boundary movement and short-rest coalescing. It explicitly
does not delete every intra-note control slice shorter than8ms.
Current opm_target_pruning.omit_short_gates removes only the selected key
pairs; opm_output_normalization changes surviving writes' target timestamps.
The renderer groups same-tick pitch/uniform-volume states, but retains other
ordered register writes. Thus temporal collapse and write deletion differ.

MSX normalization does contain an additional OPLL intra-note short-state
omission rule: note_normalization.py around265 drops runs<=44samples when
more than one run exists. This is about1ms, not8ms. Its docs describe this
separately from clock fitting; this is not a generic PSG/SCC/OPM8ms deletion
rule. The x68k8ms whole-gate policy is therefore not identical to that MSX
OPLL short-state treatment. Do not transplant it or silently broaden the
existing policy without a source-linked target projection decision.

No source/code changes, private conversions or tests during this inquiry.
