# GFII bank listening and candidate PCM WAV previews — 2026-10-21

## User evidence

User attachment c76a5e3b-369a-4bd3-8d38-94836de1aadc reports GFII05/07/08
now play with PCM. GFII04/06 still show PCM buffer shortage. User considers the
change a substantial improvement; GFII07/08 do not sound strongly wrong despite
many capacity omissions. Other songs' listening outcomes are not newly asserted.

Command: export_mdx.py local_only/opm_segapcm/vgmrips.net/GFII with best-effort,
force, no-vgm and no-song-loop. All10 generated. GFII01/02 have no PCM summary.

| Song | Route | Candidate samples | Retained | Capacity-omitted samples / playbacks | Published PDX bytes |
| --- | --- | ---: | ---: | --- | ---: |
| GFII03 | bank | 251 | 96 | 155 / 208 | 163852 |
| GFII04 | mix | 22 | 22 | 0 / 0 | 1362412 |
| GFII05 | bank | 571 | 96 | 475 / 603 | 188761 |
| GFII06 | mix | 22 | 22 | 0 / 0 | 1281216 |
| GFII07 | bank | 149 | 96 | 53 / 80 | 224581 |
| GFII08 | bank | 307 | 96 | 211 / 531 | 154401 |
| GFII09 | bank | 126 | 96 | 30 / 31 | 159625 |
| GFII10 | bank | 74 | 74 | 0 / 0 | 160729 |

04/06 hit part_resource_limit: per-voice capture exceeds16M frames, selecting
whole-song mix. This is an implementation resource guard, not a chip/driver
buffer limit. Large warning lists in the bank cases concern96 unique PDX slots,
not source block count,16M capture frames or measured native buffer capacity.
The old statistics show tracksP and omittedplaybacks0 incorrectly: tracks read
the wrong CSV column, and capacity omissions were separate from normal/voice
omission rows. Fixed display reads mdx_track/target_track and unions force rows;
projection and sample selection are unchanged by these display corrections.

## Current user request

Export the cut PCM IR as listening WAVs under a pcm_blocks subdirectory, retaining
all candidate blocks so near-duplicate material can be heard and future merging
considered. First focus SegaPCM. Do not average/merge samples yet. User's later
PSG direction is structured drum/phrase units -> PCM, other parts -> OPM.

## Saved implementation

SegaPCM normal conversion now saves all candidate samples before capacity
pruning, with pre_adpcm waveform per source view and decoded_adpcm per encoded
sample/rate. Candidate samples omitted by force remain listenable. Stored assets
and final playbacks remain separate. No normalization or second application of
onset gain. Stable ROM views therefore play at the common asset scale; baked
trajectory variants already contain gain changes. This is not a native capture.

Output: song-output/_diagnostics/<stem>/pcm_blocks/. assets.csv and playbacks.csv
link sample/view IDs, hashes, source events/channels/gates, use counts, rates,
retained/omitted status and omission reasons. Exact rational rate and WAVinteger
rounding are separate. Full encoded-nibble previews include padding and full
asset length, not just a song's shortened gate. Native audio and source State /
Segments are retained separately.

A diagnostic CLI scripts/export_pcm_blocks.py reads existing .pcm_samples.csv,
.adpcm and .pcm_segments.csv; no VGM rerender, compilation or playback. Saved
IR can reproduce decoded previews only. Its saved_ir namespace is separate so
it cannot delete nonrecoverable pre_adpcm comparisons from normal conversion.
Current PDX ownership/hash must match before saved retained status is trusted.
Manifest hashes protect modified/unowned WAV/CSV output; obsolete files are
removed only if previously generated and unchanged.

Public design docs updated. Unit tests cover observer fidelity, continuation /
seek/mute behavior, gain profile, metadata, force omissions, WAV decoding/rates,
ownership, existing-IR export and target allocation. Full authored public batch
SEGAONE/SEGABANK/SEGAMIX/SEGALOOP generated4/4 MML+MDX/PDX+WAV, no VGM replay.
SEGAONE/SEGABANK have both stages; mix cases have decoded previews. Headers and
frame counts independently checked. Artifacts and inspection JSON are under
outputs/diagnostics/pcm_block_previews_20261021/.

No new local_only VGM conversion, native listening or private regression was
performed this turn. Attempted lookup of current saved GFII07 PCMIR in outputs
found none; therefore no claim of locally generated GFII07 preview files. User
can rerun normal conversion or use saved-IR CLI against their diagnostic folder.
No staging, commit, push, source-fixture modification or private-data publishing.

## Deferred work / next allowed direction

1. User listens to candidate blocks; compare event identities, duration, rate,
   pitch, phase and gain/envelope before proposing perceptual merges.
2. GFII04/06 capture needs bounded/streamed per-voice processing, not an arbitrary
   guard increase or a claim that more PDX slots makes mix smaller.
3. Actual internal sample loops still unsupported on bank route. Localized
   bank/mix hybrid and adaptive target rates remain proposals.
4. After bank work, read msx_vgm2mml field_notes/docs and relevant mucom notes in
   depth, compare musical note/drum units, SW-envelope extraction, repeated-note
   structural loops, volume/pitch ordering and instrument/envelope state rules.
   Macro absence in MDX alone is not an explanation. User explicitly requests
   this audit before PSG block selection/PSG+OPM hybrid implementation.

Do not equate per-segment slices, software-volume runs or ADPCM chunks to musical
notes/drum units. Existing PSG native phase/shared noise/EG model and source
intermediates must stay intact. Work on mucom and enhancer remains suspended.

## Final focused verification

63 related authored unit checks pass: bank/parts15, native renderer6,
volume profile7, allocation10, capacity8, statistics2, WAV10 and saved-IR CLI5.
The public4-pattern batch and independent WAV header/frame inspection completed.
A saved-IR CLI invocation against public SEGAONE generated its separate decoded
preview; SHA256 of the original normal WAV/CSV/manifest files remained identical.
Architect review found and prompted the saved-IR namespace fix and source-view
usage-label correction; focused static rereview found no remaining blocker.

Normal candidate preview path remains as above. Saved IR output is specifically
pcm_blocks/saved_ir/. Pre-ADPCM usage now filters by proven event view_id; absent
complete mapping is labeled encoded_sample instead of asserting view identity.
This turn changes no native audio interpretation for the WAV feature itself.
