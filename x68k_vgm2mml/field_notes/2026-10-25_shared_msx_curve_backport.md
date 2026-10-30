# Shared MSX curve backport checkpoint

# Shared PSG/SCC source-curve interpretation

The requested native routes now reuse the MSX performance and fixed-tone
continuation engine before structural planning. Adapters explicitly use44100Hz
sample coordinates; the inherited MGSDRV compatibility route is unchanged.

Supported continuation combines inactive HW-reset partitions, unchanged-period
PSG release continuations, and compatible fixed-pitch rising/decaying volume
curves. Ordered zero-time pitch/mixer/HW/noise changes, silence, unknown source
time and SCC waveform changes remain protected. Native Segments are retained.

This does not import MGSDRV envelope IDs, its32-slot bank,60Hz envelope holds or
MGS-specific tie compensation. Existing target pitch, channel and PCM policies
still apply. Inferred complete sounding units are not recovered source score
Key-On/Key-Off or release commands.

Original performance and projected continuation CSV/JSON use an explicit
44100Hz clock. Existing note/source membership and target dumps remain.

Active routes: PSG/SCC -> OPM, including the existing SSG-to-PSG adapter; PSG -> PCM packaging.

PSG PCM keeps continuous emulator rendering/filtering. Interpreted groups
nominate complete-group packaging cuts before the existing chunk limit only
when no channel group straddles the cut. Hard loop/span endings remain fixed;
without a safe candidate, existing chunking is retained. Optional input defaults
leave other waveform backends unchanged. The bridge records preferred_groups
and packaging_boundaries. This is not a new reusable per-drum PCM bank or a
promise of smaller PDX/runtime-buffer fit. Derived last-state extensions remain
separate from original vgmticks_end and are reported in projection settings.

## Validation and ownership

4 new native/packaging tests,2 existing PSG PCM,5 OPM performance,5 SCC projection tests passed.

User requested minimal validation duequota; user handles local_only/listening.
No private fixture reruns, no external native compiler/emulator marathon, no
README edits, no stage/commit/push. Shared source clocks are explicitly44100Hz;
MSX remains60Hz by default. Pure engine copies retain source evidence, adapters
keep existing target conversion. Inspect original and projected dumps separately.

Public CLI check: scale_rom1.vgm -> PSG-to-OPM MDX MML succeeded with --dump-passes; artifacts are under outputs/diagnostics/curve_backport_20261025/psg_opm. No private regression was rerun.

Final review: silent-channel rest groups are retained in PCM diagnostics but cannot block complete sounding-group cuts. Authored packaging coverage includes a long overlapping rest.
