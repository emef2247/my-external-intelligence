# Reconstructed musical information to direct VGM: route audit

User has not finished HuC/BIKKR regression, but prioritizes the common direct
VGM-output foundation because future MML sizes/driver buffers are a concern.
Request now is identifying required routes, independent of HuC, not implementing
a writer yet. Public scoped audit: docs/reconstructed_vgm_routes.md.

Reviewed canonical converter, PSG/SCC PerformanceIR, target period projection,
loop-first envelope selection, OPLL melody/rhythm/voice code, normalization,
software-envelope tie compensation, drum enhancement, HuC adapter, force scope,
loop metadata and actual MGS replay helper. Native PSG/SCC/OPLL and all mixed
combinations are first; optional inferred OPLL rhythm overlay and HuC static
SCC+PSG are integrations into the same plan. OPM/OPNA/DCSG -> MGS currently not
implemented (reviewed force omissions do not constitute support); separate
OPLL grid converter may follow later.

Key gap: final accepted normalization/control boundaries/short-state and gate
choices are partly text-renderer decisions. Rebuilding VGM from early Segments
would bypass the processing user wants. Returning HuC mapping VGM is too early;
MGS-to-VGM helper depends on compilation and does not solve capacity. Extract
shared musical execution plan then branch MML/direct chip writes. Preserve
chip-specific global resources rather than independent generic note lists:
AY noise/HW envelope resets; SCC shared RAM/live wave updates; OPLL global user
patch/rhythm mode/shared percussion registers. Native rhythm Segments contain
attacks, not all release timing; ordered trace evidence remains necessary.

Architect review agreed scope/order and emphasized semantic curve decisions vs
lossless MML packaging/envelope-bank capacity, shared global operations, guarded
native/inferred rhythm coexistence, and independent source->plan, plan->VGM,
plan->actualMGS verification. Keep accepted/original timing, deterministic
same-time ordering, explicit song-loop intent and re-entry. Phrase loops/macros
remain compact in plan but finite repeats expand in VGM; size need not shrink,
MGSDRV runtime buffers cease to be a prerequisite. No new conversion/test runs,
README changes, fixture writes, commit/stage/push in this audit task.
