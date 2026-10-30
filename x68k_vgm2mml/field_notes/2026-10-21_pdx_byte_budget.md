# Table-inclusive PDX byte budget (2026-10-21)

## Request and implementation

User requested enforcement of the conservative 1,200,000-byte PDX footprint,
with reuse of existing streams/assets when exceeding it, and asked whether
all candidates are made before the 96-slot decision.

Current order is native PCM analysis / target candidate assets and playbacks,
quality-bounded sharing, then budget representative selection, then target
bindings and PDX packing. All candidates exist before selection, but are not
all packed into the final PDX. This is not a bound on conversion working memory
or time. Repeated plays of one retained asset consume storage only once.

`pcm_capacity.py` centralizes operational policy:96 slots,65535-byte verified
sample scope,768-byte table,1,200,000-byte total. This is a chosen target policy,
not a universal PDX hardware limit. NanoDrive8 decoded5MB workspace is excluded.

`budget96` now selects representatives within both slot and residual byte
budgets. Strict sharing happens first. Selection considers usage/timbre coverage;
only candidates whose byte cost fits compete. Onset repair frees enough payload
before reserving an audible representative. Existing immutable payloads are
reused; no truncation, averaging, resynthesis, phrase extraction or demixing.
Every replaced event is evaluated directly against its final representative
with actual quantized onset gain and checked gate. Source samples, playback
IDs/times/rates/pan, native evidence and IR are retained.

Applies to eligible native MSM6258, OPNA Delta-T and SegaPCM target assets,
including SegaPCM mixed chunks that already reach the common reducer. PSG PCM
is outside approximate sharing but still gets the final size guard. Invalid
source and unsupported reset/rate/control evidence cannot be fixed by a budget.

`similar`/`off` do not silently gain stronger approximation: normal oversized
projection blocks with capacity/pdx_byte_capacity. Explicit force may omit a
whole new sample that would exceed the byte cap; later reuse of retained
samples continues without extra cost. Smaller later assets can still fit.
Parallel force allocation accounts bytes before occupying a target voice.
Internal source-only parallel preflight skips the total-byte check; final
binding still enforces it. Source validation precedes representative selection.

PDX writer checks bound payload plus table before invoking the helper and
checks actual packed-file length afterwards (including padding). Oversized
packed output is removed and fails; it is never a successful output. MDX
compiler/offset, voice allocation and native playback remain separate checks.
No Rust helper behavior change or dependency rebuild was needed.

## Diagnostics

Merge JSON:budget_byte_limit,budget_input_pdx_bytes,budget_output_pdx_bytes,
byte_budget_satisfied, plus existing forced replacement and per-event errors.
Plan timing JSON:pcm_pdx_byte_limit,pcm_pdx_estimated_bytes,
pcm_pdx_estimate_within_budget. Force capacity rows:pdx_byte_capacity with
actual/limit and original sample/playback IDs/times.
Console/results CSV:planned before/after bytes and actual PDX bytes/limit/fit.
Actual fit remains unmeasured without ownership/current-export binary evidence.
Source candidate counts remain unknown for nonmerged paths rather than being
invented from final target bindings. See docs/pcm_bank_reduction.md and
pcm_pdx.md; CLI help/README describe the cap.

## Verification

253 focused public/authored checks passed in55.1s across test_pcm*,test_segapcm*,
test_opna_pcm*,test_export_mdx and test_export_report. Includes 12 budget tests,
7 new byte-capacity tests and4 statistics tests. Actual external packer accepted
exactly1,200,000 bytes; +1 rejected; malicious/changed packed padding exceeding
the limit removed. Force overflow reuse/small later admissions, source errors,
parallel8voice admission and preservation of sourceIR/events are covered.

Authored target stress case (quality sharing deliberately bypassed to isolate
capacity phase):20assets*65535B ->18existing representatives; all20events kept,
0omissions, no sourcehash changes. Table-inclusive1311468 ->1180398B. Actual
MML+typedMDX877B+PDX1180398B produced. This is target-capacity evidence, not
musical similarity, valid parsed native VGM or listening certification.
Artifacts/CSV mappings:outputs/diagnostics/pdx_byte_budget_20261021/inspection.json
and authored_source_samples/playbacks.csv plus BYTE20.pcm_bank_merge.*,
BYTE20.pcm_bindings/projection/target_commands.csv and final MML/MDX/PDX.
The diagnostic uses explicit authored IR; no raw VGM provenance is fabricated.

Actual canonical export of public opm_segapcm with default budget96, best-effort,
--no-vgm --no-song-loop:4/4 success. PDX sizes1792,4668,8568,8960B; result CSV
records byte limit1200000 and actual fitTrue. Includes bank and mix routes.
No private conversion, listening or local_only regression; user owns those.

No Git stage/commit/push. Prior unrelated public changes and external notes
preserved. True multibank output, decoder-continuous streaming slicing, PSG
musical cutout audit and memory/time-bounded generation remain separate work.

## Completion follow-up (2026-10-22)

The final byte-capacity file passed8/8 after an extra unused-parallel-assets
check (254distinct focused cases across runs). Architect review completed with
no material blocker. User regression/remaining MDX-offset failures are recorded
in2026-10-22_pdx_byte_budget_user_regression.md.
