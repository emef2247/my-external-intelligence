# Finite SegaPCM bank milestone and user regression — 2026-10-21

## Implemented scope

Added segapcm_bank.py, auto/bank/mix CLI strategy, immutable whole finite-view
assets, per-playback duration/gain/pan and shared P-W target allocation. Source
States/Segments/ROM/runtime evidence is retained. Fixed128 amplitude scale;
MDX volume gain law and physical decoder reset remain unverified assumptions.
Fractional restart address and source rate are part of view identity. Equal
encoded payloads can additionally share a binding. No per-gate clock stretching.
Only finite one-shots with stable controls/ROM are supported initially; other
capabilities cause explicit whole-song mixed fallback. This does NOT establish
traditional bank behavior for arbitrary real SegaPCM songs.

Final allocation uses shared target ticks; release precedes attack at equal
ticks. Force drops whole ninth gates without stealing. Size/slot omissions
occupy no lane, voice-only omissions consume no bank slot, and unreferenced
assets are not packed. Source evidence stays intact. Assessment records actual
sounding tracks, including none for all-omitted output.

## Public verification

SegaPCM22 tests (new bank8), multi-track10, existing PCM46 and force-capacity8
pass. Rust20 passed after helper changes; release helper rebuilt. Authored
SEGAONE/SEGABANK/SEGAMIX/SEGALOOP CLI batch generated4/4 pairs with --no-vgm.
SEGABANK: three playbacks, one asset/binding, two overlapping P/Q tracks, with
separate duration/volume/pan. Intermediate mappings and typed artifacts were
inspected. No private fixture conversion or playback was performed by agent.
Diagnostic output: outputs/diagnostics/segapcm_bank_20261020/auto/.

## User regression read-only diagnosis

Attachment ed05f646-56c4-42c1-8688-45911191d07d contains the user's commands and
results. GFII2/10, TBLADE0/12, TBOUR0/8 reported success. Commands omitted
--no-vgm. Existing result CSVs/errors confirm most failures are
pcm_replay_unavailable after MDX/PDX generation: pinned soundlog0.15.0 cannot
preserve legacy ADPCM holds/stops/resets for diagnostic VGM replay. Example
GFII03 generated MDX1065 bytes and PDX270848 bytes in diagnostics despite the
listening export not publishing the pair. Do not remove the conservative replay
guard or describe this status as PCM bank conversion failure.

Separate failures: TBOUR04 blocks at compiled MDX track7 offset >0xfffe;
TBOUR05/06 conversion exceeds600seconds. TBL11 force retains96 samples and
omits93 samples/playbacks due slot capacity. These do not follow from VGM replay.

All existing Sega-bearing strategy JSONs selected mix:
- GFII03: song_loop_held_gate;04/05/07/09: zero_delta;
  06/08/10: held_control. GFII01/02 have no Sega PCM target artifacts.
- TBL01..08: held_control;09..12: sample_loop.
- TBOUR01: song_loop_held_gate;02/04/05/06: zero_delta;
  03/08: held_control;07: sample_loop.

Therefore retained sample counts in these runs refer to mixed chunks, not
successful new source-view banks. Bank capacity improvement for these real
songs remains unachieved. User acknowledged diagnostic replay distinction and
will retry with --no-vgm. Await user local_only results. Do not rerun them or
change timeout defaults. Future meaningful work needs broader per-voice held
control/loop handling or a justified hybrid, separately reviewed and tested.

## Repository state

Public sources/tests/docs saved; no staging, commit or push. Existing unrelated
Japanese originals preserved. External private notes have unrelated untracked
mucom notes; preserve them. This note is separate from the public repository.
