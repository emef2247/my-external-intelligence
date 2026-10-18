# x68k shared-code synchronization (2026-10-16)

User requested synchronization before extending OPM/PCM/PSG/SCC -> OPNA/MUC.
The current x68k force-conversion checkpoint remains independent of target MUC
implementation. Existing uncommitted MUCOM PCM interoperability and diagnostic
changes were preserved. All edits remain in the working tree; no stage/commit,
push or branch operations.

## Synchronized scope

Common source dependencies already matched: OPM/OPNA/MSM6258/SegaPCM models,
VGM reader, musical_structure, ordered_timing and opna_adpcm_playback. Synced
new continuous DELTA waveform, shared MDX waveform packaging, force omission
registry/helpers and PDX capacity selection. Updated inherited MDX conversion,
PSG/SSG projection, exporter and TXT diagnostics. Source native IR is retained.

26 selected source/test files match x68k by normalized text, except two copied
force CLI tests add explicit --target mdx. The normal MUC main(argv) wrapper
is byte-for-text preserved; only the inherited CLI prefix is synchronized.
Protected native opna_muc, pcm_mucom, mucom_aux, mucom_fm and export_mucom hashes
were unchanged from the starting working tree. No MUCOM normalization changes.
Audit in ignored outputs/diagnostics/x68k_force_sync_20261016.

## Target boundaries and next work

--force is connected to inherited --target mdx and export_mdx only. It is not
implemented for default MUC/export_mucom. Do not silently accept/ignore it or
apply MDX noise/EG muting to native MUC SSG features. PDX96 slots/verified65535
bytes per sample are not MUCOM32 slots/256KiB payload/four-byte alignment.
Native MUC PCM held controls and exact source-byte bank path remain unchanged.

No new full OPM+SegaPCM, OPM, OPM+MSM6258, PSG or SCC -> MUC route was added.
Mixed OPM still requires explicit --pcm-only for supported PCM extraction.
Future PSG projection maps directly to OPNA SSG, not PSG -> OPM -> OPNA.
OPM8 -> OPNA FM6 excess-channel omission requires the earlier explicit-option
policy. Target-specific support and losses must be diagnosed separately.

## Minimal evidence

Nine authored checks passed: OPNA and PSG force CLI/strict retry cleanup,
97th PDX sample omission with later bound reuse, all-omitted FM MML handling,
continuous DELTA pause/end and live pan/level, native MUC default option
forwarding, MUCOM PCM packaging, native held PCM controls/source preservation.
No private fixtures, broad regression, compiler or listening checks were run.
README links focused sync/MDX-force docs; MUC current support is not overstated.
Final read-only architecture review found no functional blocker. A mistaken
nonexistent normalization-module name was corrected in docs/handoff/manifest.
Real mucom_normalization.py has a clean Git diff; no pre-sync hash is claimed.
