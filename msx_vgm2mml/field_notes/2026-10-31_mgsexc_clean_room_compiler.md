# MGSEXC: a clean-room MGS compiler with no 16 KB pool, byte-identical to MGSC where MGSC succeeds

Date: 2026-10-31
Status: hard facts (differential testing against the oracle)

## Context

Follow-up to the buffer-limit analysis: the 16 KB ceiling is
MGSC's creation limit, so a clean-room compiler can remove it.
MGSEXC (py/mgsexc.py in msx_vgm2mml, branch claude/mgsexc) was
built from public documents only (MGSC111.DOC, DSA mgs-format.md)
plus black-box probing of observable compiler outputs.

## What happened

- Differential result: 379/379 public fixture MMLs and 26/26
  Salamander MMLs that MGSC compiles are byte-identical between
  MGSC and MGSEXC. The two Salamander songs MGSC cannot compile
  (02, 06) compile to 25,088 / 21,888 bytes and play back through
  the stock driver in libkss, with register behavior in line with
  a known-good MGSC-compiled control song.
- Encoding rules that documentation alone did not settle, all
  measured: `#alloc` values never affect output bytes (only the
  set of allocated tracks matters; an allocated-but-empty track is
  emitted as a lone FF). A note whose length equals the current
  `l` state uses the short 3n form. Accidentals adjust only the
  pitch-class nibble: c- wraps to B, b+ clamps to B, and no octave
  command is emitted. `\` detune is stored negated on PSG/SCC
  tracks (1-8) and as written on FM tracks. `@e` on a PSG track
  compiles to the patch-change form. Loop offsets: `[` payload =
  pos(']') - pos('[') - 3; `]` payload = (pos('[')+1) - pos(']');
  `|` payload = (pos(']')+1) - (pos('|')+3).
- Rhythm track: instrument letters accumulate into one event
  bitmask; `r` clears it; a length, ':' or end of text flushes it.
  A group left open at another command is an error. Rest events
  are zero-bitmask events. Per-instrument volume is
  45 (target<<5)|vol with target order b,s,m,c,h = 0..4; relative
  volume is 47 (target<<5)|(offset in 5-bit two's complement).
- MGSC reads each source line into a 255-byte buffer: longer
  lines are truncated and the rest discarded, and when any line
  overflows, the output file's first byte becomes 0x00 instead of
  'M'. Output files are zero-padded to a multiple of 128 bytes.
  The binary header carries a trailing zero word after the 18
  track offsets (first track at 42, not 40).
- @v packing measured parameter by parameter: byte2 =
  KL_mod<<6 | TL; byte3 = KL_car<<6 | DT_car<<4 | DT_mod<<3 | FB;
  DT is a single bit.
- The 48-byte tempo frame table has floor(14400/T) one-bits in
  384, but its generating rule resisted fitting; all 1991 tables
  (tempos 57..2047) were extracted from oracle output and ship as
  compressed behavior data (~1.7 KB) until a formula is found.

## What to do next time

- Byte-identity is the acceptance gate for any MGSEXC change:
  run scripts/check_mgsexc.py over the public fixtures and a song
  corpus before trusting an edit.
- Inputs MGSC rejects are outside the parity contract; MGSEXC may
  accept (extension) or reject them, but never let a parity-class
  input diverge.
- The 255-byte line behavior is part of parity, not a bug to fix:
  the converter already emits short lines, so extension songs are
  unaffected.
