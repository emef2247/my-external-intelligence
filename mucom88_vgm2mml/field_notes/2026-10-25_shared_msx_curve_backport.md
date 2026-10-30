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

Active routes: PSG -> OPNA SSG and MUCOMSX PSG/SCC interpretation feeding SCC -> OPNA FM.

## Validation and ownership

9 direct PSG/MUC,14 MUCOMSX source,5 SCC OPNA and11 public/authored MUC conversion tests passed.

User requested minimal validation duequota; user handles local_only/listening.
No private fixture reruns, no external native compiler/emulator marathon, no
README edits, no stage/commit/push. Shared source clocks are explicitly44100Hz;
MSX remains60Hz by default. Pure engine copies retain source evidence, adapters
keep existing target conversion. Inspect original and projected dumps separately.
