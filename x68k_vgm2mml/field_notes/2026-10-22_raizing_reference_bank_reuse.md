# Raizing reference bank reuse versus long mixed PCM (2026-10-22)

User requested rechecking from_Fujix/Raizing/ArmedPoliceBatride and BattleGaregga
after discussion of seven MMDSP ADPCM-buffer warnings and mslug DAC streams.
Read-only metadata/encoded-command analysis. Prior PDX SHA256 rechecked against
current files; all41MDX byte lengths matched prior census. No waveform decode,
conversion, compilation, native playback or tests. Prior static scanner scope
applies:no finite repeat/song-loop expansion or loop-carried state simulation.

| Set | Shared PDX bytes | Banks | Nonempty assets | Median bytes | Max bytes | <8192B assets | MDX count / largest |
|---|---:|---:|---:|---:|---:|---:|---|
| ArmedPoliceBatride | 718130 | 4 | 114 | 5321.0 | 38677 | 95 | 26 / 16613B |
| BattleGaregga | 475413 | 1 | 74 | 3514.0 | 52748 | 56 | 15 / 13868B |

Neither bank has a sample>=60000B. All inspected PCM rate operands are4;
encoded-length duration at nominal15625samples/s has median0.681088s for BR
and0.449792s for GR; these are duration estimates, not gate/audibility metrics.
BR has at most4 PCM tracks containing notes (P–S), GR2(P–Q); not measured peak
polyphony. BR bank0 empty;1/2/3 contain47/32/35assets. GR bank0 contains74.
Current generated target still one96-slot bank; reference multibank does not
authorize silently changing that target or imply it solves payload volume.

## Concrete encoded reuse examples

- BR_BBAR.MDX:22unique sample refs,1248encodedPCM note commands,P/Q/R/S,
 14169BMDX. Referenced asset payload sum113714B within shared718130Bfile.
- BR_BST3.MDX:37refs,999encodedPCM note commands,banks1/2/3,16613BMDX.
- GR_B01.MDX:11refs,165encodedPCM note commands,P/Q,13868BMDX.
- GR_B06.MDX:8refs,261encodedPCM note commands,P/Q,12579BMDX.
 Referenced asset payload sum40739B; the actual shared PDX remains475413B.
These are stored NOTE instruction counts before repeat expansion, not acoustic
attack counts or measured total playback counts. Static command references show
reuse directly. No claim of matching original arcade ROM/sample sources.

## Interpretation and practical limits

Unlike supplied mslug VGM DAC streams, these are actual MDX+PDX sample-bank
examples. The useful pattern is many short assets retained once, with timing,
sample selection and volume on multiple PCM tracks. Current seven-warning
outputs are standard/mix/P-only and mostly~62500B/~8second chunks. Target
budget sharing can substitute whole chunks but cannot recover original short
instruments. Improving upstream per-voice part/loop/resource interpretation
is the relevant route to comparable short-asset banks. Adding banks alone
addresses slot count, not memory; fewer long chunks alone does not reducebytes.
Current converter already produces song-local banks; per-song referenced byte
sums here are a useful comparison metric, not a claim that MMDSP selectively
loads those bytes from these shared reference files.

The two reference PDX475413/718130B are larger than warning cases TBOUR01
332832B andPOWRDF02327972B. Therefore do not claim shorter median alone proves
buffer fitness. If these reference files play in the same driver/configuration
while smaller files fail, a simple total-byte threshold cannot fully explain
it. That same-environment reference playback has not been reported; no such
outcome is invented, and no environment check was made a prerequisite for
improving extraction. Prior public MMDSP whole-PDX load evidence still applies.
No definitive single-sample62500B threshold or exact driverbuffer allocation
was established. Reference playability on NanoDrive8 does not certify MMDSP.

Artifacts:outputs/diagnostics/pcm_buffer_comparison_20261022/raizing_banks.json
and prior fujix_reference_20261021 slot/track CSVs. No production code change,
Git stage/commit/push, or private payload copied into public documentation.
