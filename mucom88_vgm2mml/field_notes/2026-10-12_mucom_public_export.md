# Public OPNA patterns and native export (2026-10-12)

Ten short project-authored scores were generated through WING MUCOMVGM 0.0.5.
Each file has one L at positive time. Two-loop source facts, PCM block bytes,
voice/bank/WAV hashes and compiler identity are in the public manifest. RX21 and
third-party voices/music were excluded; PCM uses deterministic mathematical waves.

All ten VGM inputs exported musical MUC, DAT/bank dependencies and MUB using the
MUCOM88 Windows 260407 bundled native compiler. Native MUB-to-VGM replay passed
FM/SSG/rhythm/PCM control comparison over two projected traversals. This is not
listening, waveform identity or MUCOMSX certification.

Native probes corrected four assumptions: volume overwrites carrier TL; note
bases differ from MUCOMVGM; PCM table offsets are decimal 200 and native bank end
is inclusive; PCM volume compilation adds four. C128/t202 native time was measured
separately from startup. The converter now diagnoses unequal carrier levels and
other unsupported behavior. Target event CSV and projection JSON preserve the
chosen commands and source/target timing.

Native offline replay can emit the first attack at the next-loop boundary before
stopping. Comparison crops by the declared two-traversal target window, not by
silently dropping a fixed number of events. SSG source hardware period writes
are transient and reset to zero by MUCOMVGM note-on in the hardware fixture.

Inherited full-suite result from the frontend transition: 786 tests, 60 failures,
11 errors, 98 skips. Many tests expect the old MDX/MGS frontend; unrelated legacy
failures also remain. Focused tests cover the new path; the full suite is not green.

Final review added held-FM operator-change rejection, audible note-end/key-off,
effective rhythm dumps, PCM stops and raw SSG shape-trigger comparisons. Native
negative gate mutation fails; held SSG pitch without retrigger plus a same-shape
restart passes. Shared hardware-envelope channel overlaps and coincident differing
shape writes are rejected. WSL-to-Windows PCM MUC/MUB export also passed.

Final focused run: 34 tests passed, including four real native tests covering all
ten public patterns, deliberately wrong gate endings, held FM pitch changes and
held SSG pitch/retrigger behavior. A snapshot made only from staged files passed
30 tool-independent tests. Git diff whitespace checks passed. External tools,
local generated MUBs and outputs were excluded from staging; no commit was made.
