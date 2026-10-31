# C140 mixdown route implemented; BF03 exports with zero omissions

Date: 2026-10-31
Status: hard facts (measured); level calibration still open

## Context

Implements the mixdown direction of
`2026-10-31_c140_mixdown_and_level_calibration.md` on branch
`claude/c140-mixdown`: `--c140-projection {bank,mix}` (default bank).
`c140_bank.render_mix` renders every voice on the native frame clock
into stereo accumulators (value * left/510, value * right/510 per
frame, the per-side form of the dynamic-gate level baking in
`render_gates`; not-yet-dynamic voices are held raw and flushed with
their initial levels, mirroring `make_dynamic`). `project_mix`
packages the mix through the existing waveform route (mono fold,
chunked independent ADPCM encodes, identical-byte chunk sharing).

## What happened

- Unit cross-check: mix output matches an expectation rebuilt from
  `render_gates` captures for static, dynamic and panned authored
  gates, plus a channel filter. A 10-simultaneous-voice authored
  fixture converts through `convert()` with no force and no omissions.
- BF03 (71.1 s, 2,682 gates): exported 1/1 in 3m21s. PDX 555,360
  bytes, 9 chunks, clipped samples 0, `pcm_omitted_playbacks.csv`
  header-only, mapping gate coverage 2,682/2,682. The bank route on
  the same song plans 1,290,904 PDX bytes, budget96-cuts to 975,924,
  and still blocks without force (694 gates omitted with force).
- Native mix level: L/R RMS 5253/5296, peaks 23975/26762 (no native
  clipping). Render alone takes 122 s vs ~600 s for the bank route.
- Chunk sharing measured, not assumed: all 9 chunks are unique.
  Byte-identical sharing is active but a single through-composed
  traversal has no identical 8-second repeats; song-loop traversals
  reuse chunks through the MDX loop instead. At 555 KB no further
  sharing machinery is justified.
- Failure worth keeping: the first BF03 export silently used the
  bank route. `export_mdx --c140-projection mix` worked through
  vgm2mml.py and the plain batch path, but `run_batch`'s listening
  delegation rebuilt its options dict and dropped the new key, so
  the listening layout (the default CLI path) fell back to bank.
  A new option is not wired until every options-rebuilding layer
  forwards it; a regression test now asserts the key reaches the
  listening batch options.

## What to do next time

- New CLI options must be forwarded through `run_batch`'s listening
  options dict as well as the direct options list; test the
  listening path, not only the direct one.
- Remaining stage: level calibration against the pinned libvgm
  reference (C140 vs YM2151 RMS ratio) in the environment that has
  `.tools/c140-reference`; apply the result as one scale where the
  mix is packaged. Until then the mix level is the uncalibrated
  /510 convention.
