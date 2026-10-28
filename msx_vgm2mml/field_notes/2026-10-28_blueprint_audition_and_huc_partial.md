# 2026-10-28: blueprint audition, loop layout and HuC force subset

The user paused the MUCOM musical-layer port to prioritize small MSX changes.
README and user run scripts/logs were preserved. No private music regression
was rerun and no commit or push was requested for this checkpoint.

## Silence diagnosis and approved change

The October24 MGS definition-terminator correction remains present in
mml_sync.definition_allocation: reserve definition bytes plus the driver
terminator. Do not add unrelated allocation padding to cure silence.

Authored MGSC/libkss reproduction showed a PSG blueprint without volume had
peak0; v15 made it audible. Default OPLL was nearly silent. The user explicitly
approved fixed initializers at each blueprint track beginning. Melodic PSG uses
q8 /1 v15; OPLL uses q8 @1 v15; SCC uses q8 @0 v15 plus one fixed square wave.
Source dynamics, patches and waveform definitions remain documentary and are
not used in score repeat equality. These target audition defaults supersede
the previous policy of leaving every blueprint completely uninitialized.

Unedited authored blueprints compiled and produced nonzero audio: PSG peak4544,
SCC1903, OPLL2047. This is libkss playback, not a browser listening claim.
Source allocation score atoms remain free of volume/timbre commands.

## Loop presentation

Ordinary and blueprint MML share a whitespace-only formatter. Every ]N ends its
line. Leaf loops can remain inline. Loops containing child loops close on a
separate line at their opening indentation; following commands use current
depth. Track prefixes remain on every physical score line. Token equivalence,
idempotence and sync-comment depth are tested.

## HuC native reference versus target

Published tracks/<stem>.vgm contains original HuC source musical operations,
not SCC conversion. Diagnostic _huc6280_target/<stem>.vgm contains projected
SCC/PSG/OPLL writes. Failure of the target did not imply that all source voices
were unmappable: prepare previously received no force flag and one unsupported
channel blocked the whole input.

Force now forwards into HuC prepare. A bounded whole-channel subset search
retains the largest ordinarily validated nonempty subset. Active DDA/LFO or
unsupported waveform channels are excluded; source issues and mixed/unknown
commands cannot be bypassed. Omitted channel indices/scope are explicit in
projection JSON and console output. Source CSV and native reference remain
unchanged; target event IDs are remapped to original command ordinals. No
empty score is claimed as success when all channels are unsupported.

Authored mixed wave+DDA input retained channel0 and explicitly omitted5;
original44 HuC writes survived native VGM while target VGM contained SCC writes.
A loop located on a removed write still maps to the same source-time boundary.
Focused suite:82 tests passed. Evidence is ignored local output under
outputs/diagnostics/blueprint_silence_20261028, including fixed/verification.json
and authored native/target/control files. These outputs are not committed.

## Paused work

MUCOM has only an incomplete/unvalidated musical_hal package copy; no MUC
integration or independent reference/blueprint export is claimed. Resume that
port only after the user requests it. These MSX changes are uncommitted.
