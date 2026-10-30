# Budget96 and shared PCM8 bank checkpoint (2026-10-21)

User explicitly chose at-most96 target samples as the default priority, accepting
approximate replacement rather than silent capacity drops. They deferred WAV
orphan recovery and accepted their budget regression before requesting stage3.
No local_only VGM conversion or listening was performed by the agent.

Budget96 is saved: strict quality sharing first, deterministic usage/timbre
coverage for remaining assets, direct source-to-final comparisons with actual
quantized gain/gate, forced error/loss statistics. All target sample references
fit the selected<=96 inventory. Original IR remains. Eight budget unit tests
include onset preservation against delayed prefixes. AC excursion128 is an
experimental target audibility safeguard; no universal perceptual claim.

112 related public tests passed after budget implementation. Authored99 assets
produced96 bindings/99 events/no omissions withoutforce, typed MDX4002bytes and
PDX13056bytes. PublicSegaPCM4/4 exported defaultbudget96. User then requested no
more budget regression and moving immediately to shared MSM6258/Delta-T support.

Common reducer now accepts native MSM6258 and OPNA projected PCM after original
reset/supply/rate/control validation. Internal validation permits capacity
omissions so source>96 can reach reduction; final allocation/userpolicy still
runs. A separate target wrapper adds approximation evidence without rewriting
native transfers, supplied byte counts, controls or source dumps. No eligible
bounded asset retains legacy capacity behavior. PSG PCM remains excluded.

Finite Delta-T adapter decodes/converts complete independent stable samples once
per exact source bytes/rate/profile, independent of gate/level/pan. PCM8 onset
volume approximates level/256 once. Native memory/sample/reset provenance stays
separate; bank assets/events CSVs and bridge route identify the new stage.
Held rate/gain/pan changes,repeat,zero/unresolved controls and resource scopes
retain old continuous/legacy behavior. Source/sample hash corruption fails.

80 focused public checks passed after common implementation, including4 native
bank-boundary and7 Delta bank tests, legacy PCM/rate/profile/control tests.
Two old OPNA expectations were aligned: mocked downstream has no final PDX
binding evidence, and projected asset wording is no longer transcode-only.
Actual authoredDELTA_PAIR and publicreset_pan_hold exports each succeeded1/1
throughMML/MDX/PDX. Delta2events share1complete37-byte asset withvolume128/137,
PDX805bytes; nativeMSM2events share1asset,PDX1280bytes. Inspection JSONs under
outputs/diagnostics/pcm_shared_bank_20261021/; budget artifacts under
outputs/diagnostics/pcm_budget96_20261021/. Architect read-only review found no
remaining boundary blocker. This is not native listening certification.

Still separate: total bank/playerbuffer,8voice andcompiler limits; no demixing
of native software mixes; no source-loop reconstruction or true multi-bank
PDX extension; PSG sounding-unit cutout audit deferred. No WAV orphan recovery
implemented per user preference. No Git staging/commit/push; external note
repository is separate and all unrelated changes were preserved.
