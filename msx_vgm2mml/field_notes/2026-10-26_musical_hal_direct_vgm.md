# Shared musical HAL and direct VGM: implementation checkpoint

## User scope and branch
User prioritized reconstructed musical output as VGM independent of MML/MGS
capacity, then described future TX802x2/FB-01x4 MIDI and additional cartridge
mappings after a shareable MGS baseline. Existing vgm2tx802 may supply future
OPLL/FM instrument conversion. MIDI/expandedchips are NOT implemented here.
User requested all current uncommitted work including HuC on a new branch:
created codex/musical_HAL from main; no commit/stage/push; README unchanged.

## Layers and critical boundaries
New musical_plan.py contains frozen half-open units and typed piecewiseconstant
controls with original/source and accepted44100 sample times, event/Segment
lineage and global operations independentofnotes. Native state remains separate.
Source PerformanceIR and native OPLL Segments are retained/dumped; initial
msx_standard mapping uses existing acceptedperiod/semitone decisions. Original
pitch/timbre parameters and source clocks remain for later target profiles.
Global AY noise/HWreset, SCCsharedRAM and OPLLpatch/rhythm operations must NOT
be flattened into independentnote lists. MMLbank/macrolimits are laterlowering.
Musicalpatterns describe alreadyexpandedtimeline; do not expand them again.

musical_conversion.py orchestrates native analysis, existing score/curve
interpretation, pure accepted timing, target plans, nativeglobal events and
optional acceptedpercussion. musical_vgm.py writes combinedA0/D2/51 with rational
declaredclock timing, boundGD3/EOF/loop metadata and command-source mapping.
export_vgm.py publishes flat tracks, originals, diagnostics,manifest andresults,
with subprocess timeout and foreign/modified output protection/rollback. No
MGSC/Node/MMLparser/rendering required; PATH-empty tests prove this. Canonical
--output-format mml|vgm|both sharesdecisionobjects; both writesVGM first.

Pure AcceptedNoteDecision is shared by existing PSG/SCC MML +directexecutor.
NormalizedMelody decisions extracted from note_normalization into musical_timing
are consumed by both. Final MML pendingPerformanceIR objectidentity checked.
MML-only envelope packagingfallback decomposes retainedbasepartitions only after
piecewiseconstantvolume timeline equality; sourceunitmapping +sharedclock retained.
Failure to lower acceptedclock/layout is explicit, not silentreinterpretation.

## Traps / corrections validated
1. get_ticks is ceil(time*60) then tick1->0, not nearestround. Preserve this
legacyinterpretation and raw samples separately. Accepteddeclaredtempo uses
exact rationalstep conversion; inferredmeasuredlattice tolerance isn't an absolute
8ms guarantee afterlong integer-tempo drift. Source/acceptedtimes are visible.
2. Softwarecurves use logicalnote boundaries; internalholds stayframe-based,
independentof32-bank packing. Clipattargetend is reported, not silently lost.
3. OPLLsemitone FNUM table measured in actual authoredMGS runtime:
172,182,194,205,217,230,244,258,273,290,307,325 at MGS o4/block3.
High/key byte is written beforelow1samplelater: inspecting attack alone gives
previous/intermediatepitch. Stable fullregister state is necessary.
4. Native rhythm ch6-8bank must not be overwritten by accepted melodicquantization.
AtmodeON restore retainednativebank beforemode; whileON onlynative rhythmcontrols
are authoritative. AtOFF restore accepted latentmelodicstate, including data that
native legacySegmentbuilder excludes duringrhythm. Mode resourceunits are explicit.
5. Architect repro: normalized zero-length ON followed bypositiveOFFrest became
heldON because onlyzero rows were replayed. Fix retains complete exacttimestamp
group +settlingstate/sourceorder. Authored actualserializedrestgate test passes.
Do not replay quantized shortpositiveFM slices to undo FMfiltering.
6. Inferred drums respectnativeheldinstrument priority, clips atnativeattacks,
preserve/restorenative volume nibbles including interveningnativevolumeupdates.
When no nativepercussion exists, zero-resetpitch wouldsilenceBD/TOM. Actual authored
MGS init measured lowregs16/17/18=32/80/192, high26/27/28=5/5/1. Label these target
initialization, not sourcefacts. Sharedacceptedgroups feedbothoutputs.
7. VGMloopre-entry checks actualaudibleregister dependencies overtwo traversals,
no reset ofunrelatedheldvoices. Phase/hardware equivalence is not claimed.
8. Existing MML sustainbit loss remains: shared/direct retain it, canonicalreport
backend_losses recordsaffectedunits. MMLstaticrhythmlayout cannotdropaccepted
melody7-9; backendfails explicitly andkeepsalreadygeneratedVGM. Driverenvelope/
notedispatch timing differences remain backendfacts; don'tclaimbyteidentity.

## Verification and evidence
153 selected authored/public/actualMGS/reference checks pass,0fail/error/skip.
Existing public MML baseline test passes. 60 supported publicinput conversions:
PSG13,SCC13,OPLL19,PSG+SCC2,PSG+OPLL1,PSG+SCC+OPLL12 allsuccess. Final OPLL19
rerun afterzero-pulsefix succeeds. Earlier minor mockedmissingworker-reportdetail
failure corrected; final acceptance is clean. Do not run unfiltereddiscover:
legacytest_local modules consumeprivatefixtures. No broadprivate regression.
Only BIKKR06 intentionallycheckedonnewVGM route, success; userownregressionongoing.

Optional scripts/musical_vgm_reference.py builds unchanged local pinnedlibvgm
emu2149/emu2413/k051649/panning underignoredtools; metadata pinscommithashes,
sources/dependencies/wrapper/binary/attributions. Authoredmixedaudio and perchip
frequency equations checked, plus publicprefixnonzeroaudio. SCCmixedresampling
nearest; no audioequivalence/hardware claim. Upstreamcommit sameHuCprofile
bad50c571db45b872304bdcd0603dd2214f0663e; no network setup required locally.

Ignored results outputs/diagnostics/shared_vgm_20261026 include summary.json,
acceptance_tests.log/json,mml_public_regression.log,final_public,drum_probe,
independent_audio.json,BIKKR06. Docs/musical_hal.md is publicusage/scope; README
leftuntouched. Both public and externalprivate changes unstaged/uncommitted;
unrelatedprivate notes preserved; no private push authorization.
