# Continuous PSG to MDX PCM (2026-10-18)

## Authorization and priority

User firstrequestedPSG/SCC-only selectors andPSG->PCM, then clarified priority:
finishselectors, defaultFM+additive/autobank/README, savecheckpoint, thenresumePCM.
Firstcheckpoint savedandpublic-authoredchecked beforePCMimplementation. Userowns
privateregression/listening. No privateinput reruns. No agentGitstage/commit/push;
userstaged filesduringwork, stagingpreserved. Latestworkingtreeedits mayremainunstaged.

## Architecture

--psg-output opm|pcm defaultopm; PCMrequiresbest-effort, structuredMDXchannels.
PSG/SCCsource evidence/musicretained; PSGPCMremovesPSGfromOPMreservation. PSG-only
supportszeroFMnotes; SCC-only+PSGPCM rejected. Runtimecore pinnedexternalMIT
emu2149 441f3dc295a9b82db16921461f0678b483db8304; authoredC bulkadapter `.tools` build.
Setup standalone scripts/setup_psg_pcm.sh or setup_tools --with-psg-pcm.

OnecontinuousAYtype0 core44100Hz; original0xA0writes appliedonceinorder, including
sameR13value. Shared3ch noiseLFSR/EG. No resetatMSXnote/SWvolume boundaries or
readerloopjump. Corequality1 rateconverter/highpitchsuppression, zero-periodDAC
limitations, mono/wiringflaginterpretation andunemulatedI/O remainexplicitmodel
assumptions. SourceWAV/orderedwriteCSV/gates/JSONinputandcorehashesretained.
Gates use expandedoccurrenceeventIDs consistently; originalID/address/iteration
are traced through orderedCSV, not mixed into same namespace.

Targetview ACcoupling20Hz +centered63tap Hamming-sinc lowpass cutoff.45targetRate,
commonPSGgain(0,1], thenexisting waveform->MSM6258packager. Sourcewaveunchanged.
Filtertails/prenechooutsideamplitudeactivegatescropped, gain/bandwidth/phase,
mono/linearresampling/ADPCMreset/loopfreeze all explicitlosses. No claimedexact
AYanalog output or guaranteedantialias/memoryfit. ActualADPCMbytes dedup only;
per-notepercussion reusewouldrequireexplicitphase approximation, deferred.

Opt-in source-exact intermediate OPM writer onlythisroute, sorts originalsample/
physicalchannel/writeID, rejectsnegativewait. Roundedperformanceaudit remains
separatefrom exactserializedOPMmapping/nativeprojectedtraces. FM/PCM shareoriginal
EOF/loop andsingleexpandedtraversal, overrideexacttimeline into canonicalOPM
pipeline. Validator includesPCMchunkboundaries, fallback6samplebound orconfigured
normalizationbound; commonpositivePCMclock/evennearestcount<=1rateSample retained.
SourceIRnotrescaledtospoofEOF. Existing opmconversion with source_passes_outdir
keepsrawPSG/SCCoriginals separate; PCM/MML/PDXoutputs atnormalroot, hashescoherent.

## Saved-before-check evidence

25focusedchecks passed:5SCCsource/voice/filtering,4actualAYcore runtime,2exactwriter/
filter/profile,9existingPSG/SCCtarget,5existingperformance. Workerrechecked4runtime
cases afterprovenancecorrection. No broad/privatebatch orlistening.

AuthoredAYLOOP sourceperiod/nose/EG withloopat997 andfirstEOF13001, continuous
readerreplayEOF25005, emittedL13001. GeneratedMML310bytes, MDX117, PDX5188.
All3hashes matchassessment; maxoriginal->finalboundaryerror207samples (<352).
Sharedclockmultiplier65; eachPCMchunk nearest-even durationerror<=1selectedrate
sample. Source10musicalunits retained. Rawsourcehash verified. Source-end equals
exactserializedOPMEOF25005, not metadata-only replacement.

Public-derivedMIX:publicSCCvolume_sweep plusauthoredAYtone; SCCadditive8 and
PSGPCM ultra_low typedMDX/PDXexportsuccess. Rootoutputs/diagnostics/
psg_pcm_20261018/{pcm_only_loop,listen_mixed}; earlierflagsautobank under
outputs/diagnostics/source_selection_20261018. No privategamecontent inpublicdocs.

Architectplanreview prevented pernotephase resets andbroadcastR13tripling,
identified oldintermediatequantization barrier andexactwriteorderingrisk.
Completionreview caughtmixedIDnamespace/corehashlabel; fixed/tested andrereviewed.
Filtertailcropping explicitlyadded. No remainingreviewblocker. docs/psg_pcm.md,
README, thirdpartynotice andhandoff updated. MUCOMSCCtargetstillfuture; no changes
toMUCOMtoday, no timewindow or optionalVGMexport restart.
