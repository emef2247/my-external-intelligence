# Similar PCM bank merge implementation checkpoint (2026-10-21)

## User result and cause

After the quota interruption, user's GFII export with best-effort/force/no-vgm/
no-song-loop succeeded only for FM-only01/02. Every03..10 PCM conversion_failed
was caused by ModuleNotFoundError: pcm_bank_reduce. Integration had been saved
before its new core module. This is an incomplete checkpoint error, not evidence
against the source data or PCM route. New module now saved and exercised.

## Implemented target policy

See public docs/pcm_bank_reduction.md for full stable implementation boundaries.
Defaultsimilar/off applies SegaPCM-derived finite banks and existing mixedchunks,
after align_to_clock, before96slot/byte/eightvoice selection. Source PCM IR and
all original samples/WAVs remain. Direct existing representatives, onset gain
composition viaactualPCM8table, everydistinct finalgate/gain checked, nooffset/
ratewarp applied. Provisional100cent bound withknownuniqueROM/rate evidence or
tonalestimate; unknownnoise explicitlyunknown. Short spectral/envelope method
is intentionally approximate and need notmatchphase/waveform. Longerexisting
chunks retain decodedwaveformerror<=.08; no phraseextraction or newsourceinterpretation.

Mapping/errors/rejectioncounts in pcm_bank_merge JSON/sampleCSV/playbackCSV.
Source vs retained vs represented IDs explicit. Recovery isactualcurrent-minus-
baseline playback IDs withsameclock/options, alongside newlyomitted andnetdelta.
A/B diagnostics separate fromcandidateWAVs: actualcheckedtargetgate,quantizedgain,
max24pairs/16s perhalf, gap.2s, no normalization, explicitcap/rate/ownershipdata.

## Verification

104 relevant public authored tests passed; core30 andpreview11 included.
PublicSegaPCMbatch4/4 success bothsimilar/off throughMML+MDX/PDX, bankandmixroutes.
Authored gainpair source->MML/MDX/PDX test checksbinding2->1, volumesdifferent,
unchanged sourceIR/rawsamples/originalWAVhashes, newA/Bmanifest andactualgatebasis.
Architect foundA/Bsourcegate mismatch; fixedtousecheckedtargetframes andtested
extendedtail. Addednewlyomitted/netdelta toavoidcallinggrossrecoverypureimprovement.

SavedGFII03derivedIR numericprobe(noVGMconversion/compile/listening):
251samples->60;390937bytes->123779;191samplesmerged;332playbacksremapped;
265screenedcomparisons;2.7178seconds. Methods145waveform_gain_rate/46spectral_envelope.
This uses decodedassets/source-timegates, notfinalMDXclock; capacityunevaluated,
recoveredplaybacksunknown. Worstwaveerror1.952 isphase-sensitive residual,
notvolumeerror; auditoryqualitynotvalidated. Needuserregressionbeforeclaiming
actualretention/buffer/soundimprovement. OriginalGFII07savedIRplannedprobeabsent.
Ignoredoutputs: outputs/diagnostics/pcm_bank_merge_implementation_20261021/.

No private VGM full conversion or native listening performed. User owns these.
No Git staging/commit/push. External repository remains separate; preserve
unrelated prior notes. GFII04/06 wholephrase reuse possiblebutunconfirmed.
Source capturebudgetfallback andPSG/MGS-derived musicalunitaudit remain deferred.
