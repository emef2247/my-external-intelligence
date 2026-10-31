# MGS data over 16 KB breaks on MGSEL real-hardware playback

Date: 2026-10-31
Status: hard rule

## Context

MGSEXC removes the MGSC 16 KB creation cap, and v2.1.0 of msx_vgm2mml
made MGSEXC the default compiler. The MGSDRV API specification states
the driver itself handles performance data over 16 KB.

## What happened

On a real MSX (MGSEL 3.01v, OPLL on a SoundCore SLOT EXG, SCC-I
Snatcher cartridge), converted songs whose .mgs exceeded 16,384 bytes
misbehaved: the tracks whose data crosses file offset 0x4000 dropped
out or turned to noise, other channels froze, and the song never
terminated (MGSEL's P.count kept incrementing in silence).

Controlled test: one song that plays fine at 8,960 bytes was recompiled
with its music streams byte-identical, padded only with unused @s/@e
definitions to 17,152 and 20,608 bytes. Both padded files failed the
same way on the same setup. All files decode cleanly offline (every
track FF-terminated, finite loop counts), and libkss plays them.

So the limit is not in the MGS format or in MGSEXC's output; it lives
in the classic playback path (MGSEL 3.01v loading/playback at least).
Spec-level driver support is not ecosystem support.

## What to do next time

- Keep MGSC the default compiler (done in v2.1.1); MGSEXC stays opt-in
  via --compiler mgsexc.
- Treat >16 KB MGS output as not safely playable on classic real
  hardware setups, however valid the data is.
- When a release depends on a format limit, verify on the real
  playback path, not only against the spec and emulators.
