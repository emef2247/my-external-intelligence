# HuC6280 BIKKR initial implementation

## Scope and source contract
User authorized implementation starting with BIKKR. Single native HuC6280
periodic static-wave source now feeds ordinary SCC + AY PSG, through a retained
target VGM into existing sounding-unit / loop / macro / SW-envelope code.
README unchanged; no fixture changes, commits, staging or pushes. OPLL spill,
noise, active DDA/LFO, mixed sources and dynamic audible waves are unsupported
and fail explicitly even with force. Do not describe the broader design as
implemented. Full catalog regression and listening remain user-owned.

Native py/huc6280.py retains ordered B9 writes, immutable state, six-channel
positive source intervals, 32-u5 RAM versions / known flags, source gains and
phase/reference assumptions. Source profile tested against unchanged libvgm
c6280_mame.c at bad50c571db45b872304bdcd0603dd2214f0663e, SHA256
f342dd818a7b4bf0afe15f12320e62d85ff38f5ebf78ba2cc4d49b8e6f02eb4b.
Optional scripts/huc6280_reference.py builds an ignored local wrapper and
compares RAM/register/index/counter/gain/audio on authored sources. Not hardware
proof: LFO/noise/shared-pointer uncertainties remain explicitly labeled.

## Projection and useful corrections
Ordinary SCC has 5 voices / 4 independent memories, shared last pair. Static
allocation examines every positive audible interval, requires observed complete
RAM, and permits 5 only with a matching shared pair throughout the input.
Overflow PSG accepts only exact two-level, 50% duty square shape. No future-wave
preload; target upload only when observed complete and about to be audible.
Do not write disabled partial-wave versions over a held shared-memory partner.
Source Segments are retained separately from target-derived PSG/SCC Segments.
.huc6280.{allocation,controls,gates,target_writes}.csv carry source-event links.
Enable/audibility gates are not inferred complete score notes.

Crucial clock verification: actual MGSDRV/libkss export uses AY and SCC header
clock1789773. SCC output is 2*headerclock/(32*(P+1)), not headerclock/(32*(P+1)).
Measured actual PCM SCC P427=261.342Hz, AY P427=261.931Hz; same octave.
Native HuC frequency =3579545/(32*(P or4096)). Explicit Hz projection produces
SCC Psource-1 and AY Psource for typical clock values, but never blind-copy
source periods without a clock profile. Bounds checked at target, source intact.

Gain: native integer reference 1.5dB attenuation table applied before stereo
linear mean and 4-bit target quantization. SCC linear, PSG approximate3dB;
PSG relative-mixer factor0.7968 from authored steady SCC RMS1794.64 vs AY2252.37
(SCC square80/78). Wave amplitude is retained, not independently normalized.
PSG square DC/phase, target reset behavior and absolute gains are approximate.
Quiet rounding-to-zero remains visible and does not release allocated resources.
Repeated volume writes must NOT rewrite target frequency registers: doing so
reset SCC dividers and inflated target Segments / note partitions. Fixed this.

Two architect reviews caught replay-cache hazards: (1) redundant loop-entry
period writes may restore a different EOF period; (2) source pitch restored while
muted was absent from target, so a redundant loop-entry enable could restart the
old target pitch. Retain dependency writes in loop body; first active source
control/balance/global write per channel restores pitch. Never clear whole cache
at loop entry or reset unrelated held voices. Guard native second-traversal
positive state/wave dependencies (including wave upload pointer), then independently
replay projected target AY/SCC commands twice and compare audible period / gain /
wave / mixer state at waits. Unsupported nonperiodic dependencies fail explicitly.
Source/target phase equivalence is not claimed by these state checks.

Batch MGS playback export remains real; status projected_source replaces false
OPLL 0/0 equality for actual B9 sources. Header-only HuC clocks do not select it.
Generated _huc6280_target directories excluded from source rediscovery. The bridge
protects unowned intermediate files by previous target digest and leaves native
diagnostics on target failure. Existing publication ownership rules retained.

## Scoped private observations (not public fixtures)
Only BIKKR06 and BIKKR01 intentionally converted for acceptance; broad user
regression not run. Initial general unittest discovery unexpectedly entered old
local-only tests (two compression and Aleste catalog early cases); interrupted
at Alest208, then selected authored/public modules explicitly. Do not repeat
unfiltered discovery when only public/scoped private verification is intended.
No private originals changed or redistributed.

BIKKR06:158895ticks,2621writes,1163nativeintervals,onewave(all6),5SCC+1PSG;
MML5121bytes,MGS2432bytes; fixed stereo FF, low5volume12..26, attenuation5..19.
BIKKR01:1648531ticks,12007writes,6013nativeintervals,onewave(all6),5SCC+1PSG;
MML24827bytes,MGS14592bytes. Both compile and actual MGS->VGM export succeed.
Both max projected pitch error0.000484cents, zero positive gains rounded to zero.
Actual 4.5s audio probes:06peak4343/RMS1118.20;01peak6076/RMS1271.79.
These are nonzero playback checks, not listening or complete acoustic equivalence.
All six authored projected parts' active pitch registers checked in actual MGS
output; source-native reference, target replay and compilation are separate tests.

Artifacts outputs/diagnostics/huc6280_impl_20261026/ contain native06, gain_probe,
BIKKR06,BIKKR01, verification.log and analysis_ja.md. Keep private data ignored.
Final verification counts are recorded in handoffs/current.md after completion.

## Final review and verification
A third architect reproduction showed phase-dependent enabled RAM writes may
change only on the third loop, despite two matching traversals. Added a guard:
loop wave RAM writes require disabled source plus an observed in-loop DDA falling
reset; enabling invalidates this pointer proof. Third-loop reproduction rejects;
explicitly reset disabled upload accepts. Do not claim finite replay proves
arbitrary phase-dependent periodicity. Architect confirmed identified blockers
addressed after this guard. Public project knowledge records the stable rule.

Selected authored/public suite77checks passes with no skips/failures/errors;
focused HuC suite33passes including added third-loop and valid-reset cases.
Original general discovery is intentionally not reported as a passing fullsuite.
Public code/docs/handoff and this private note saved; neither repository staged,
committed or pushed. Existing unrelated private note changes preserved.
