# SegaPCM runtime and PDX bridge (2026-10-13)

User authorized PCM target implementation after FM-only listening. Private
regression remains user-owned; no private input conversion or compiler run.
No Git staging, commit, push or branch change.

Observed source State/control Segments and ROM blocks are unchanged. Separate
runtime IR uses explicit VGMPlay-compatible RAM FF/fraction-zero assumptions,
clock/128 frames,16.8 addresses, equality before fetch, live register/ROM
updates, natural stop, hardware loops and source loop state carryover. Sparse
missing reads use neutral80 with counts; no declared-size ROM allocation.

Target bridge mixes stereo to mono, clips with counts, linearly resamples to
F4/15625Hz and uses independent MSM6258 ADPCM chunks up to8seconds, split at
MDX L and EOF. Preserve short packaging tails; they are not source gates.
After selecting the shared FM/PCM clock, encode against final scheduled gate
duration with nearest even sample count. Original intervals remain inspectable.
Source ROM bytes and target ADPCM bytes are separate. No MSM6258 supply/reset
observation is fabricated. Strict policy blocks loss, best-effort permits it.

Known limitations: stereo folding, no antialias filter, clipping/requantization,
independent predictor at seams and fixed recorded traversal when looping.
Physical decoder/reset/playback equivalence unverified. Existing96-slot and
65535-byte per-sample scope remain. Native render bound32Mframes/1Mevents.

Validation:14 authored source/runtime/bridge tests passed. SEGAONE, SEGAMIX,
SEGALOOP public authored fixtures generated MML/MDX/PDX/TXT successfully using
normal typed PCM helper. Inspected gate CSVs/native waveform size, final chunk
mapping, P-track L, and all PDX payloads exactly matching target ADPCM bytes.
SEGAONE naturally stops at1.048576seconds. All three selected16.64ms/tick;
final target duration error0F4 samples. Outputs: outputs/segapcm_pdx_check.
Native listening remains user-owned. No private regression run.

Next: user full-PCM regression with --pcm-policy best-effort. Keep timeout180;
compiler capacity optimization and new compiler integration remain deferred.

Focused review: natural-stop fraction now clears before restart, matching the
VGMPlay reference. Added and passed an authored stop/restart case. No further
blocking issue identified in best-effort bridge/clock integration review.

