# The MGSDRV track-buffer limit lives in MGSC's ~16 KB workspace, not in the MGS format

Date: 2026-10-31
Status: hard facts (black-box measurement against MGSC.COM via mgsc-js)

## Context

Where does "Track buffer full" actually live: the MGS format, the
compiler (MGSC), or the driver (MGSDRV)? This decides whether a
clean-room extended compiler can raise the limit while staying
format-compatible. All probes are observable behavior of the stock
MGSC 1.11 embedded in mgsc-js; no source was read.

## What happened

- `#alloc` is a capacity pool, not file layout. Salamander 01's MML
  allocates 16,000 bytes in total and compiles to an 11,264-byte MGS;
  unused capacity is not padded into the file.
- Raising the allocation total is refused with "Can't allocate" at
  the `#alloc` line. Bisected maxima: Salamander 01 accepts a total
  of 16,292; Salamander 03 accepts 16,264. The cap is therefore a
  fixed workspace minus a small per-song overhead (28 bytes differ
  here; title length was tested and is not the overhead). 16,384
  (0x4000) is the natural workspace-size candidate.
- Redistributing the same total across tracks compiles; the check
  is on the sum (plus overhead), so there are two distinct failure
  layers: a bad distribution can fail a song whose total fits, and
  no distribution can save a song whose compiled total exceeds the
  pool. Per-track overflow ("Track buffer full") is the first layer
  surfacing when one track outgrows its share of a capped pool.
- The MGS itself is densely packed data with 16-bit-scale offsets;
  no 16 KB wall is visible in the format. Nothing in compiler
  behavior proves the driver imposes the same limit.

## Official grounding (added after reading the Gigamix spec)

The MGSDRV API specification (ver 3.15, Gigamix) states the split
directly: performance data is at most 16 KB; data exceeding 16 KB
is in fact processed correctly, but MGSC creates only up to 16 KB.
So the cap is a documented compiler creation limit, and the stock
driver side is officially not the wall for uncompressed data.
Separate caveat: MGSARC-compressed data needs a 16 KB decompression
work area, a different constraint. The Gigamix article also notes
MGSC's source is not secured even by the maintainers, which is why
black-box measurement was the available route; DSA's independent
mgs-format.md documents the MGS layout.

## What to do next time

- Treat the ~16 KB ceiling as an MGSC workspace property. A
  clean-room compiler can in principle emit larger format-valid
  MGS data; whether stock MGSDRV/players actually play >16 KB data
  is a separate playback experiment (libkss or real hardware) and
  is the first milestone to verify before relying on it.
- Do not cite 15,000 or 16,000 as the limit: those are this
  project's chosen distribution budgets. The measured compiler cap
  is ~16.3 KB of allocations for these songs.
- When a probe definition is needed, check the actual MGSC error
  text first: an invalid parameter (e.g. envelope number 99) is a
  different failure from "Can't allocate" and invalidates pool
  conclusions drawn from it.
