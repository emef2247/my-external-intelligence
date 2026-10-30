# x68k STATE24 regression and DCSG OPM milestone

## User steering and corrected listening evidence

User explicitly selects24ms default for fm-state-ms based on command reduction,
not loop-count growth. Keep normalization8ms independent. Batch stage timeout
600->1200seconds due GFII05. User now says all compared KNIGHT STEP/STATE
outputs differ incorrectly from source VGM. This supersedes the earlier
no-perceived-difference impression (including48ms). The historical sweep still
measures artifact capacity/structure; it does not establish acoustic fidelity.
No new private input conversion, compilation or listening was run here.

## Saved regression audit

Read9logs and corresponding saved results/PCM assessment JSONs. Totals:
BOSCON12/12,DRSPRT22/28,TBOUR6/8,Granada17/18,OUTRUN10/12,ThunderForceII21/21,
Nemesi9519/25,SionII0/8,GFII9/10.22reportedPCMblockers split into17source
interpretation blockers and5MDXcapacity failures mislabeledPCM. Sion01-08 and
DRSPRT01/21/28:stopped/unknownplay data supply plusreservedpanbits80/88;
Nemesi9501/03/04/15/18/25:MSM6258register17 unsupported. Force capacity cannot
resolve these source semantics. GRANAD09,TBOUR05/06,OUTRUN02/11:typed assessment
mdx_capacity_exceeded / trackstartoffset0xfffe, MML/PDX retained. DRSPRT04/08/12
separate compilation failures have sameoffsetissue. GFII05 timedout600seconds.
Outputfolders:GFII_STEP24 andSion_STATE24. Report/CSV saved under
outputs/diagnostics/state24_regression_20261023/. Saved regressions unchanged.
Batchstatusnow uses typedassessmentcode toclassifyMDXcapacity ascompilation_failed.

## DCSG milestone

Native dcsg.py copied unchangedfromMUCOM. No AYtargetinterpretation reuse.
DCSG exactsample period/attenuation views reuse sharedMSXextract_notes and
volume-run membership. Musicalattacks inferred, sourcehasnokeyregister.
Single/dual tones mapA-C/D-F. OPM7noise allocated perinterval; lowerinstance
priorityduringoverlap, strictreject orforceomitoverlapthenresume. Fixedwhite
noise nearestOPMrate withLFSR/phase/reset/spectrum lossdiagnostics; periodic/
tone2linked unsupportedstrict/forceomitted withoutremovingtones. Unknownlatch/
monostereowrite strictreject; forcewrite+unknownstateinterval diagnostics.
SourceEvents/States/Segments remain. Two-pass declaredloop analysis preserves
latch, noise, chipinstance; LbeginsfirstEOF with replaylineage. Exactsource
serialization toprojectedOPMVGM thenordinaryOPMstructure /24ms filter /8msclock.
Provenance labelsDCSG explicitly. No semantic envelopeclassifier added.

Architect reviewed: intervalnoiseownership, latchloopcontinuity, provenance,
unknownsourceissues. Gaps werefixed. Publicauthoredtests andmmlxcompile run;
see repository handoff and outputs/diagnostics/dcsg_public_20261023/verification.json
forfinaltestcounts/artifacts. No privatefixturesaddedtopublicgit.

Next: userlocal_onlyregression/listening, thenagreedMSXcatchup andconcrete
oneattack+internaltypedcurve prototypeusingMGSDRVexplicitSWenvelopes. No generic
x68kframework placeholder orblanket24mspruning ofsourcePSG/SCCcurves.
