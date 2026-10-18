# OPM simultaneous six-voice allocation (2026-10-18)

User GENPEI01..15 conversion_failed with --force because old projection counted
whole-song used sourcechannels and rejected >6, independent of actual overlap.
User explicitly requests simultaneous6. Earlier fixedomit-only authorization is
superseded forforceauto; explicit fixedomit/error remain. Timewindow feature held.

Implemented target-only DynamicChannels helper, preservingnativeOPMState/Segments.
Defaultauto choosesdynamic only when >6songactive +force; <=6 auto staysoldfixed
for outputcompatibility. Dynamicexplicitallowed, withforce requiredforcapacity
loss or possible releasecut. Sourcegate from0->nonzero mask, held/partialchanges
retaineduntilfull0. Firstsixadmittedvoices priority; newovercapacitywholegate
omitteduntilKeyOff, thennextattackcanretry. No stealing/latejoin/loudnessheuristic.
KeyOff is notsilence: owner remainsforreleasecontrols untilnewownership. Reuse
force recordspossiblereleasetruncation. Ordered same-sample onbeforeoff canomit
newgate conservatively, reasonopm_same_sample_order_capacity explicit.

Atclaim emitcompleteobserved rawsourcevoice24operatorfields+AL/FB/pan/pitch+
4neutralSSGEG withcachebypass. Incompletevoice rejected, notfilledfromoldowner.
UnmappedDT2/noise/LFO snapshot checkedatclaim; diagnosticsnameactualsnapshotreg,
triggerwrite preserved separately. Pitchhigh/low andkeymasks remainexistingrules.
AllocationCSV acquisition/reuse/keyoff/dropstart/end plusnativeevent/gate/time/ch
andold/newslot owner;JSON staticchannel_map is now labeledEOFonlyindynamic.

Architect preplanreview: source2traversals alone doesn'tprove allocatorperiodic.
Implementloop_start entry/EOF audit owners(all),keys,dropped. Unstablemapping
normallyerrors; forceomit_song_loop and renderexisting2traversals finite onall
FM/SSG/PCM. No resetatL/inventedheldattack. SourceIR/replayloopmetadata retained.
Existingmusicalseam/normalization/voice/inputguards stillapply. DynamicPCM
projectionalreadybuilt has source-loop evidence; finite rendering uses common
loop_startNone andsourceend unchanged, not pretendPCMsource lackedloop.

Authored checks6new pass (plus6existing projection):8serialchannels all8gates,
wholegateoverflow/partialKey/noLateJoin/reentry withlatestTL, preservedreleased
controls, unseenDT2snapshotforce/incompletevoiceguard, same-sample conflict,
actualstableloopandunstablemapping finitefallbackwithMSM/commonend, CLI/batchauto
andexplicitpolicy. Newtests initiallymocked too few CLIreportfields andsearched
' L' in titleLOOP; fixedonlytestfixture/tokenassertion, rerun6pass.

Actualbatch --muc-only --force generated2authoredVGM outputsets under
outputs/diagnostics/opm_dynamic_20261017/export. EIGHT8serialsourcechannels=>
8MUCgates,all0..7claimed,2releasecutpossibilities;CAPACITY7held=>6MUCgates,
1G omission at7/6voice capacity. InspectedallocationCSV and.mucom.json, MUC
produced. No privatefixture reruns/nativecompiler/listening, userownsregression.
Files saved beforechecks; no Gitstage/commit/push/branchchanges. Preplanarchitect
review succeeded; finalreviewattemptfailed due modelquota; no approvalclaimed.
READMEshortdocslink; detailedcontractdocs/opm_to_opna.md andmucom_force.md.
