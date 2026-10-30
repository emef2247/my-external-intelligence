# Sion II / Nemesi95 MSM6258 source interpretation fix

User requested correction after the STATE24 audit, asking whether a datasheet
was needed. Main findings are in VGM virtual-register interpretation and target
preflight, not command byte framing or proof of malformed input. No datasheet
was necessary for this narrow reference-compatible fix.

## Primary reference

Pinned libvgm70e1d1fad8c03df2ce4bfb13f8599c56920dcdc8,emu/cores/okim6258.c:
reference SHA25667a4ead490281d481cab7f2836be7cc6608b7de68359234f3fb2d0bc3552a04a.
Source saved under outputs/diagnostics/msm6258_source_profile_20261023/.
Audio output uses pan &1/&2. Write dispatcher has no address0x17 handler.
Stop-to-PLAY reinitializes FIFO. Hardware buffered-data behavior not certified.
Current master and legacy VGMPlay source were also read as corroboration.
Do not confuse virtual register0x17 with VGM stream chip ID0x17 or invent a
pan/clock alias from observed data values.

## Saved private evidence (read-only)

SIONII01 saved PCM raw CSV has a STOP at sample0 and two direct supplies before
first PLAY, including one just before the first PLAY. Its pan0x88 has lowbits0.
NEME9501 saved raw CSV has694 register0x17 writes (values0x10/0x88). Their raw
values alone do not establish intended hardware routing. No local_only VGM
was reconverted or compiled; user continues those checks.

## Implementation

Native msm6258 analyzer retains raw B7 transfers/control data. Pan upper bits
become pan_non_audio_bits observations with effective low2bits. Register0x17
becomes unmapped_register_17 observation, without changing known audio state.
Other unsupported registers still block. Known/assumed-stopped direct supply
becomes direct_supply_while_stopped, separate from derived stream supply.
Unknown play-state data retain data_outside_known_playback blocking behavior.
Native source JSON identifies the pinned register-interpretation profile.

Target pcm_mdx best-effort explicitly omits stopped direct supplies and unmapped
0x17 writes with loss codes. Strict rejects these known losses. Native source
bytes remain available; active playback samples remain exact. Pan upper bits
alone are a nonfatal source interpretation observation. Runtime FIFO uncertainty
and byte-supply/decoder-consumption distinction remain explicit.

## Verification

Synthetic tests cover all low pan combinations, raw preservation,0x17 inside
active playback, unchanged active sample bytes, stopped supply followed by
reset PLAY, unknown-state rejection, strict versus best-effort. Existing
source17 tests and projection47 tests passed. An authored fixture combining
all three conditions successfully generated mmlx MDX+EX-PDX; verification JSON
records loss codes and artifact sizes. Final broader test counts are in the
repository handoff / verification JSON.

Architect and explorer follow-ups were unavailable because their model usage
limit was reached. Main session directly inspected pinned source and source/
target intermediate diagnostics; no external review result is claimed here.
No commit, staging, push or changes to MUCOM/MSX were performed.
