# MUCOM derived PCM low profile (2026-10-16)

User requested smaller PCM output choices for MSM6258 and SegaPCM; root handles
x68k Delta-T/MSM6258/SegaPCM in parallel. No private regression, compiler or
listening checks authorized/performed. No Git mutation.

Added standard|low profile, default standard, to canonical MUC wrapper and
export_mucom batch, including subprocess forwarding. Shared pcm_profiles is
identical to x68k. MUCOM computes integerDelta-N per call, not via mutable
module globals: standard18462 ->15625.390625Hz; low9231 ->7812.6953125Hz.
Actual rational rate drives resampling/counts/chunk bounds, MUC pitch/detune,
Unit structural identity and nominal duration audits. Native OPNA ADPCM bytes
and rate remain unchanged; manifest marks profile inapplicable to that route.
Source State/Segments/PCM IR retained. Capacity32slots/256KiB/four-byte alignment
and force omission/reuse rules unchanged. Low permits longer time per bounded
sample; padding/dedup/omission mean no guaranteed half-size whole bank.
Existing linear interpolation has no dedicated anti-alias filter. Smaller bank
is not a guarantee against runtime PCM-buffer shortage.

Saved code before authored checks. New3 tests pass, existing pcm_mucom11 pass.
Initial new-test failures were assertion API/status errors (Unit source_key,
batch muc_only), corrected without projection algorithm change. New tests
cover source adapters, half-rate counts, Delta-N MUC spelling/Unit semantics,
duration/interval retention, standard-after-low stability, chunk capacity,
canonical/batch forwarding, native OPNA exact MUC/bank equality.
Compared default _project output bank/items against gitHEAD for authored100ms
waveform: exact. Persistent public authored SEGAONE MUC generation and mapping
inspection under outputs/diagnostics/pcm_low_check_20261016: source span remains
0..46243 samples; standard payload8196bytes, low4096bytes. Standard valid16385
samples plus7padding; low8192 plus0padding. Nominal duration errors+0.8679 and
-1.9544 VGM samples respectively. Profile/rate/Delta-N/encoded byte fields
inspected in mappingCSV/capacity/projectionJSON; these are synthetic evidence,
not private-song/audio equivalence. Diffcheck clean.

Changed README (brief link), docs/pcm_interoperability (scope/losses), handoff,
py/pcm_mucom.py, py/opna_muc.py, vgm2mml.py, scripts/export_mucom.py and new
profile checks/shared profile module. User regression/listening is next.
