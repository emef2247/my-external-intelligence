# mmlx size options and seven PCM-buffer warnings (2026-10-22)

User asks whether MDX256KB prevents further improvement, whether mmlx has a
size option, and what TBOUR01/02,POWRDF02/05/06,GFII04/06 buffer failures share.
Read-only inspection of installed dependency sources and existing output/CSV/PDX.
No production changes, conversion, compilation, listening or tests.

## mmlx / layout facts

Installed mmlx0.2.0 is library-only, with no compiler CLI or optimization flag.
Its mdx::compile(&parsed) lowers AST to commands, without an optimizer pass.
Inspected dependency root:/home/emef220/.cargo/registry/src/
index.crates.io-1949cf8c6b5b557f/. Sources:mmlx-0.2.0/src/mdx/compile.rs:47-49,
Cargo.toml lib/autobins flags, README features. Helper calls that API unchanged.
No setting was invented or enabled. Public package metadata:
https://docs.rs/crate/mmlx/0.2.0

soundlog0.15.0 MdxLayout::calculate rejects track starts>0xfffe and tone offset
>0xffff (src/mdx/document.rs:102-132). Its builder docs describe those limits.
The helper additionally rejects combined typed MDX>65535B. These are different
from the NanoDrive8 256KiB allocation. Relocating/wrapping offsets is not a fix.
Pre-layout command compaction could help; no conclusion that all musical
outputs are impossible or that MDX universally has a256KB format limit.
Existing phrase/repeat compaction already applies; further reduction would
be implementation work, not a missing mmlx switch.

soundlog has MdxBuilder::set_lz_compressed(bool) and serialized-body LZ support
(document.rs:272-281,488-535). mmlx compile API does not expose that flag.
This is serialized storage compression after layout, so cannot fix the
pre-layout track-offset failure. Player compatibility is separate. Not enabled.

## Existing generated files

All7 reported warnings correspond to standard-profile, single-P, mix route.
Each stores a sequence of long ADPCM chunks, mostly near8s/~62500encodedB,
rather than short reusable instrument assets. MDX files are8167–21233B, well
below the reported player allocation; this is a separate PCM load problem.
All7 published PDX copies byte-match their diagnostic PDX copies. This does
not establish the identity of copies actually loaded by the user player.

| Song | PDX bytes | Assets | >=60000B assets | Route fallback |
|---|---:|---:|---:|---|
| TBOUR01 | 332832 | 6 | 5 | sample_loop |
| TBOUR02 | 1141768 | 19 | 18 | part_resource_limit |
| POWRDF02 | 327972 | 6 | 5 | sample_loop |
| POWRDF05 | 1067700 | 18 | 17 | part_resource_limit |
| POWRDF06 | 997348 | 17 | 15 | part_resource_limit |
| GFII04 | 1174924 | 19 | 18 | part_resource_limit |
| GFII06 | 1156224 | 20 | 18 | part_resource_limit |

Two sample_loop cases:TBOUR01,POWRDF02. Five part_resource_limit cases:
TBOUR02,POWRDF05/06,GFII04/06; per-voice capture exceeded its explicit frame
budget. Those fallback codes do not prove the original source is inherently
an instrumentless stream. Raising limits blindly is not a justified fix.
The existing bank analysis/capture resource behavior is an improvement target.
PDX-level representative sharing can only reuse the whole candidate chunks
produced by this fallback; it cannot recover the underlying individual drums.
All7 PDX are327972–1174924B and<=1200000. A size cap for NanoDrive8 is not
a guarantee that MMDSP/MADRV has that much resident PCM storage.

## Independent public loader evidence

https://github.com/gaolay/MMDSP/blob/master/src/MACTRL.s
LOAD_PDX reads the full PDX file length into d3; load_pdx30 (lines1064–1089)
passes that length and pointer to MADRV#$00. A negative return preserves-5,
annotated PCM buffer insufficient at the function contract. It frees the
host file buffer and clears driver PDX state after error. This public source
is evidence of whole-PDX transfer failure, not a measured single-sample
62500B limit or a diagnosis of the user-installed driver variant/configuration.
Exact resident buffer size is unmeasured. Do not invent256KB/320KB defaults.
https://github.com/gaolay/MMDSP/blob/master/src/MXCTRL.s has analogous TRANS_PDX
whole-file transfer, adding its270B wrapper and mapping negative result to-5.
Downloaded sources/doc are ignored under outputs/diagnostics/
pcm_buffer_comparison_20261022/reference_*. Content hashes were printed during
inspection. No downloaded code was executed.

## Consequences / not implemented

Candidate follow-ups:separate playback-target-specific file budget (MMDSP vs
NanoDrive8), improve resource-limited bank capture, and handle internal-loop
assets with explicit target capabilities. No global byte-limit reduction or
new target preset was chosen unasked. Small independent samples can still
produce a bank too large for a given driver; fewer voices alone is not enough.
Existing low/ultra_low profiles halve/quarter total audio for the same duration,
but double/quadruple max chunk duration (16/32s), keeping max chunk bytes near
62500. They do not promise lower per-sample requirements. In this public loader
path, reducing total PDX is the directly relevant load-time improvement.
No recommendation to arbitrary-split ADPCM bytes or reset unknown decoder state.
Other songs not in the warning list do not automatically count as confirmed
playback successes; use the user reports as scoped evidence.

Abstract comparison:outputs/diagnostics/pcm_buffer_comparison_20261022/comparison.json.
No Git stage/commit/push. No new private data copied into public docs.

## User follow-up: Fujix mslug as a solution reference

User asks whether from_Fujix/mslug, also streaming PCM, can guide the solution.
Yes: four PCM-containing VGM/VGZ files use type04 banks and VGM DAC stream0,
feeding MSM6258 register1. Stage1/3/6 use two blocks; FinalScene1P uses one.
Total encoded bytes:1034357/1090325/1061879/1131465. NeoGeoLogo is FM-only.
Stage1 blocks169394/864963B start at0/956195ticks; both starts have observed
STOP1 thenPLAY2, with streamstop94/start95 at the second boundary. Stage3/6
have analogous explicit control boundaries. FinalScene is one1131465B block.
These are long ADPCM supplies, not a short per-drum reusable bank. No MDX/PDX
is present in the supplied mslug folder, so it is not evidence of MMDSP loading
an equally sized streaming PDX or playing arbitrarily long PDX slots.

Applicable lessons: keep continuous ADPCM predictor/nibble/order evidence;
separate long supply data from play/stop/rate commands; distinguish explicit
STOP/PLAY epochs from arbitrary transport/time cuts. Existing MSM6258 analysis
already represents stream supply and reset/control separately. This is a useful
reference for native/target continuity validation, not automatically new format
support. No source reanalysis/conversion/listening was run in this follow-up;
facts come from the preserved command census and current source code.

The current MDX/PDX loader passes the whole PDX to MADRV. A few long slots vs
many8-second slots changes boundaries/slot pressure, not the encoded total or
the whole-file driver allocation need. A VGM stream supply capability must not
be conflated with on-demand disk streaming by a different playback target.
The received data does not establish disk/refill memory behavior of any player.
No claim that mslug must fit the user's current MMDSP buffer, nor that its
stream structure could recover original individual rhythm samples.

For the7-warning cases,5 bank captures hit a tool resource limit rather than
a proved intrinsic streaming requirement; improving that extraction remains
an actionable candidate. Internal-loop support is separate for the other2.
A playback-specific file budget would also be separate from the established
NanoDrive81200000B target. No backend/profile/streaming output change made.
Follow-up artifact:outputs/diagnostics/pcm_buffer_comparison_20261022/
fujix_mslug_streams.json.
