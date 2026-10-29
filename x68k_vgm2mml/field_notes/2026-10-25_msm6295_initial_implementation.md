# MSM6295 initial implementation and validation

## Scope and source profile
User authorized implementing the reviewed sample-bank design. Production route
is single-chip MSM6295 alone or OPM+MSM6295, fixed clock/pin7 and basic bank0.
No README edits, staging/commit/push or broad local_only regression. Only user-
authorized FFIGHT01/02/03 were converted; original fixtures were unchanged.

Immutable msm6295.Trace preserves raw ordered B8 writes,8B ROM blocks/coverage,
source/traversal IDs, clock/flags, observed State/command Segments and labeled
reset assumptions. Inferred assets/playbacks/accepted+ignored requests and busy
state are a separate source-runtime stage. Complete source samples cache by
bytes/nominal length/rate/reset profile, independent of volume/early stop.
Per-asset decode/resample/MSM6258 encoding feeds shared PCM8/EX-PDX backend.
No per-playback waveform capture or whole-song mix fallback was introduced.

Pinned libvgm revision bad50c571db45b872304bdcd0603dd2214f0663e:
- okim6295.c SHA25600778ea2bb630abf549ea25671e9d19df50d4ce4889be9e1fcfa6739b5ae6194
- okiadpcm.c SHA25695ad62ea67fb3f239873abf79200ce11d3d548c40022ec02da0dd4a3abaf5af1
Independent optional C wrapper/source/library hashes are recorded under ignored
.tools/msm6295-reference. Source/bank modules are independently implemented.
Oracle compares exact native stereo PCM, cursor/busy, decoder signal/step:
nine authored scheduler/control cases plus912-nibble decoder coverage.
Reset signal=-2/step0, high-first source ADPCM and full-scale signal*16 are
reference-profile facts, not target decoder assumptions. Nominal source rate
and reference integer native grid remain distinct (1MHz/132 vs7575Hz here).

Target losses explicitly include resampling/no antialias, requantization,
PCM8 onset-gain quantization, timing approximation and unverified driver/reset
acoustics. Auto exact-origin policy is best-effort; explicit strict still fails.
Four source voices occupy target PCM8 channels independently of the OPM voices.

## Source/target and unsupported boundaries
No-op basic extension initialization is retained. Active unsupported clock,
pin7/bank/NMK/unknown extension states fail explicitly. Restoring unsupported
state while idle can remain conservatively blocked. Changed ROM while any
voice is active is conservatively rejected even if ranges might not overlap;
these are MVP implementation limits, not hardware specifications.
Sparse ROM bytes consumed by playback must exist. Optional unplayed tails may
remain incomplete known prefixes. Invalid table ranges ignored by the pinned
core remain ignored source requests; missing table/data never become zero/FF.
Native reset/pending/busy state carries through actual VGM loop traversal.
Shared target song-loop continuation guard remains; no fabricated restart.

## Validation results and artifacts
21 MSM6295 tests initially passed (source5,bank10,integration4,oracle2). Further
final boundary fixes/tests and final count will be recorded below.
Public batch3/3 passed with actual compiler/packer output, including4voices,
asset reuse across gains/stops and inactive-PCM header/empty-ROM case.
Existing CLI25,export31,PCMstats4,policy6,C140integration4 checks passed.
VGM I/O checks:3 passed; existing title-space assertion fails (expects no space
after[SMS], actual has a space). Reproduced using HEAD vgm_io.py in an isolated
module. Unrelated title code/test was not changed to mask this baseline failure.

Focused private batch output: outputs/diagnostics/msm6295_impl_20261025/FFIGHT.
--no-song-loop,auto policy, noforce,default mmlx:
- FFIGHT01 success:5samples,216PCMplaybacks,0omitted,PDX14893B,MDX7716B.
- FFIGHT02 success:FM only, noPCMcommands/PDX,MDX807B.
- FFIGHT03 success:6samples,73PCMplaybacks,0omitted,PDX17533B,MDX2285B.
Request/nativeplayback/sample CSVs, targetbindings/projections, sourceclock and
issues, published binary sizes and unchanged source-copy bytes were inspected.
Initial01/03 output has no native-end beyond EOF or same-voice overlap, so the
final authored boundary correction does not alter their measured event ranges.

FFIGHT03 with default song-loop enabled was also checked separately under
outputs/diagnostics/msm6295_impl_20261025/FFIGHT03_song_loop:138nativeplaybacks,
6assets; blocks at the existing held-PCM song-loop guard. Recommend explicit
finite pass for initial user listening. No physical playback/listening performed.
Packed-byte fit and compilation do not establish native buffer fit.

## Final review findings
Architect confirmed latch/busy/decoder/cache/routing structure, but found a short
EOF case:4nibbles at7575Hz,EOF18ticks gives a reference frame end2352/101ticks,
and naive targetceil24 exceeds sourceEOF18. Keep nativeframe evidence and bound
target end byEOF/next accepted same-voice start; record clipping separately.
Also distinguish onset-grid error (<one nativeframe) from final tick rounding.
Root additionally requested native runtime dumps before target-resource checks
so a successful source stage remains inspectable after later target failure.
Fix completion and final test results will follow below.

## Next allowed action
User owns broad local_only regression/listening. Existing C140 user regression
was recorded without new analysis. Preserve C14048m budget. Do not begin other
chip support or bypass the song-loop continuation guard. Public docs and this
private note are separate repositories; no push of private notes is authorized.

## Final accepted checkpoint
Blocking fixes saved and independently reviewed: project optional outdir/stem
emits completed native IR before target-resource checks; target end bounded by
EOF and next accepted same-channel start, without modifying native frame/end
or busy behavior. Native onset-grid shift, end rounding and target clipping
reason/amount are distinct event columns. Three meaningful regressions added.
Final24 MSM6295 tests passed (source5,bank13,integration4,oracle2); all21C140
checks passed after shared integration change. Existing CLI25/export31/stats4/
policy6 passed; baseline title-space failure remains recorded above.
Authored EOF diagnostic dumps inspected: native end2352/101,frame4; targetend18,
source_end bound,6clippedticks. Artifacts under msm6295_impl_20261025/eof_boundary.
No additional private reruns after bounded-end fix: prior FFIGHT01/03 native
CSV inspection confirmed no EOF overrun or same-channel overlap in either file.
Native IR before target failure is covered by an authored resource-limit test.

Production changes are saved in the public repository working tree, including
source/bank modules, routing/policy/stats/shared conversion, export cleanup,
optional oracle, original authored public fixtures, tests, license notice and
focused English docs. README unchanged. No staging/commit/push. Existing
uncommitted handoff/design work was preserved. User owns subsequent regression.
