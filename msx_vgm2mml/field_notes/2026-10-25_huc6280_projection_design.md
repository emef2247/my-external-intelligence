# HuC6280 source support and hybrid MGSDRV target design

## User scope and work performed
User requests considering HuC6280 support in msx_vgm2mml, SCC primary with
OPLL/PSG complementary voices, and explicitly permits reading the whole
local_only/huc6280 folder to inform the specification. Investigation only:
no converter/README/test changes, compilation, playback, or fixture mutation.
No stage/commit/push. Full future regression remains user work unless scoped.

Read MSX AGENTS/project knowledge/current handoff and relevant inherited notes.
Current converter supports PSG/SCC/OPLL, and batch collector filters.vgm/.vgz.
Do not reintroduce removed MDX tools or target aliases. Preserve the established
full sounding unit -> repeat structure -> shared envelope selection order.

## Source inventory
106 valid VGM-magic inputs in6sets; clocks all3579545 and commands onlyB9,
waits/end. KATOKEN's14files have no filename extension. Input originals unchanged.
Group: files / maximum enabled-positive-gain voices / noise inputs / DDA inputs:
- BIKKR:24 /6 /0 /0
- DSPILIT:17 /6 /4 /0
- GENPE:16 /6 /6 /0
- KATOKEN:14 /6 /14 /0
- SPLTTR:16 /6 /3 /0
- DSAVER:19 /6 /1 /16
No observed positive-duration active LFO, live normal-wave writes while enabled,
DAC stream commands or second-instance/unknown-register writes. No concurrent
noise pair with different programmed rates in this corpus. These are corpus
observations, not chip restrictions or future support promises.

Ignored diagnostics: outputs/diagnostics/huc6280_design_20261025/{inventory.json,
inventory.csv,summary.json,wave_shape_features.json,inspect_sources.py,analysis_ja.md}.
Enabled/gain counts are command-based reference assumptions, not rendered audible
counts. Whole muted32-write blocks form waveform candidates; cyclic-shape
canonicalization does not reconstruct native RAM pointer or phase.98% square/
sine fits are exploratory classifiers, not product thresholds or exact mapping.
Global shape classes:1square50,5sine,72other. BIKKR eachinput observes1shape;
its spill candidates are square-compatible under this exploratory model.
Non-square spill occurs in other sets (e.g. all17DSPILIT and14SPLTTR inputs),
so PSG-only complementary mapping cannot be assumed sufficient generally.

## Target design decisions
Public design: docs/huc6280_projection_design.md. Native HuC6280 State, all6
channels/32point unsigned5bit wave RAM, channel select, period/control, stereo
master/channel gains, noise/DDA/LFO and ordered source timing remain distinct
from target. Native note/curve inference must not treat every register slice
as a note or an entire continuously-enabled song as one note. Do not apply
24ms/8ms musical smoothing to waveform/DDA transport writes.

Ordinary SCC:5oscillators but4independent waves, duech4/5 sharedRAM. Six
independent source waves may require4SCC+2complementary voices. Five SCC voices
are possible with compatible shared-wave trajectories across the full overlap;
current instant shape equality alone is insufficient. SCC+ independent-five-wave
mode needs actual MGSDRV/runtime verification; do not assume it is available.

Prioritize complex periodic timbres on SCC, square-like spill and noise onPSG,
other periodic spill onOPLL with explicit approximation. AY noise generator
is shared; distinct concurrent noise rates need a conflict/loss policy. OPLL
user patch is global despite manyMML@vdefinitions; do not fit two different
concurrent custom patches independently. Fixed presets/one compatible custom
patch are candidates, evaluating native FM envelope as well as steady spectra.
Prefer stable phrase/gate allocations, no mid-note chip hopping/fake attacks.

Projection must account for source log gain +L/R/master ->explicit mono target
gain quantization, and source/target counter/clock/zero-period/phase differences.
Initial wave amplitude map(u5-16)*8 is a target approximation, with native data
preserved. MGS structural/render adapters come after explicit target allocation,
reusing existing loops/macros/software-curves rather than another parallel MML
builder. Source and target IDs/curves/errors/capacity must haveCSV dumps.

DDA needs its own timed source-value IR; ordinary32point periodic-wave notes do
not automatically provide arbitrary-rate PCM playback. Confirm driver capability
before claiming a target DDA path. Preserve it and report unsupported/explicit
losses; never silently drop16DSAVER inputs' DDA. LFO needs an independently
checked coupled-channel profile; none is exercised by the supplied corpus.

## Suggested implementation/validation order
Native source +authored independent reference first. BIKKR06(3.603s) is an easy
6voice/SCC+PSG basis, DSPILT01(1.358s) forcomplexspill/OPLL, KATOKEN/11Miss
(3.255s) fornoise+extensionless discovery, DSAVER13(12.884s,19919enabledDDAwrites)
forlaterDDAwork. Do not rename extensionless private originals implicitly.

Authored cases:5/6distinct waves; shared SCC4/5 compatibility and a held overlap
while other wavechanges; AY2differentnoise rates; OPLLcustompatch conflicts;
HuCwavepointer/reset/DDAoff-upload vsactualDDA; stereo gains,phase/LFO and sparse
initialization. Pin source runtime and compare real MGSDRV output. Cross-chip
approximation cannot universally use register-byte-equality as fidelity proof.
No production implementation begun by this investigation.

## Primary references
VGM spec https://vgmrips.theclassicgamer.net/VGM_Specification
MAME HuC https://github.com/mamedev/mame/blob/master/src/devices/sound/c6280.cpp
MAME SCC https://github.com/mamedev/mame/blob/master/src/devices/sound/k051649.cpp
emu2413 https://github.com/digital-sound-antiques/emu2413/blob/master/emu2413.c
HuC reference source itself notes phase/LFO/noise uncertainty; implementation
must label/pin the chosen profile rather than promote guesses to hardware facts.
