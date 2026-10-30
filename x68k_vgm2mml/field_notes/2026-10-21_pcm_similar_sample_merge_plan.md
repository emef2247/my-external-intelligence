# Similar percussion PCM reduction proposal — 2026-10-21

## User scope

User listened to preview blocks and distinguishes individual percussion sounds
in GFII03/05/07/08/09/10 from phrase/stream-like material in04/06. User explicitly
defers why04/06 look streamed; do not investigate their source interpretation
in this task. User requests bold merging of similar single sounds, permitting
pitch differences below one musical step and constant loudness differences.
Use100cents (a semitone) as a provisional conservative interpretation, not a
confirmed clarification of whether the user intended a semitone or a scale step.

No production conversion changes, private VGM reconversion, tests or agent
listening. Existing metadata and GFII07 WAV numerical analysis only. Public
proposal docs/pcm_bank_reduction.md saved. Source IR remains immutable.

## Existing behavior

Exact encoded-byte equality deduplicates target samples. Stable ROM views
already externalize constant onset volume andpan; held-control variants bake
absolute gains. Approximate gain/rate/timbre merging is not implemented.
Multiple pre-ADPCM preview rows with the same encoded hash all refer to the
same sampleID; they are already deduplicated, not additional slot savings.

Metadata candidates with same exact ROM bytes hash plus fixed address/end range
but differingdelta:03 has4ranges/8assetrows;05 has12/29;07 has1/2;08 has5/11;
09 has1/2;10 has0/0. Those are potential rate variants, not confirmed merges or
canonical instruments. Corresponding gain-baked playback rows:189,242,65,282,
57,50. GFII05 has1044WAVs but no preview manifest/assets/playbacksCSV currently;
its separate bank assets/eventsCSV is available. Do not invent preview metadata.

## GFII07 numerical probe

149encoded candidates and160sourceviews. Select the most-used pre-ADPCM view
perencoded sample. Full-wave positive gain fit, zero padding rather than common
prefix truncation. Bounded rate search plus small alignment is approximate and
not exhaustive. Results are candidate pairs, not clusters or saved slots:

| Relative RMS threshold | Gain only pairs | Rate/gain/alignment pairs |
| --- | ---: | ---: |
|2%|9|19|
|5%|10|20|
|10%|22|47|

Alladditional2%/5% matches have zero pitchshift andatmost2samples onsetalignment.
Only17of47 pairs under10% choose nonzero pitchshift, withbesterrors about6.9%.
Thus loudness and tiny entry/phase differences offer concrete first opportunities.
No claim that149willfit96afterthisstrict waveform probe.

Example sample009->114: same3306frames, gain0.4921889377, relativewaveRMS
0.0001496218 (0.01496218%), correlation0.9999999888. 009has116localviewuses,
114has1. 009->017 needs1samplealignment, error0.4354%;009->104 needs1sample
andgain0.49218, error0.4361%. These are pre-ADPCM comparisons.

Single targetasset illustration: representative009decoded * actualprofilegain
0.5(raw137) versusdecoded114 has relativewaveRMS0.1112523 (11.125%). Desiredgain
0.49218894 maps to0.5, gainerror+0.007811 absolute (about1.59% relative desired).
Do not confuse11.1% waveform residual with an11.1% volume error. Independent
ADPCM encoding also changes waveform residual. No all-event acceptance/native
listening was performed. Strong pre-encode fit alone is not final acceptance.

Ignored diagnostics:
outputs/diagnostics/pcm_bank_merge_plan_20261021/metadata.json,
waveform_probe.py,gfii07_probe.json,gfii07_pair_candidates.csv,
gfii07_target_gain_example.json. No private waveform payload copied into docs.

## Proposed target stage

OriginalPCMIR -> groups -> representative/event remapping -> force capacity ->
P-W/MML/PDX. Preserve every originalsample/view/event, separatelossreport and
WAVs. Integrate before96slot selection, so events previouslydropped can refer to
representatives. Report finalactuallyrecovered playbacks separatelyfrompotential.

First gain-only/full-wave material variants, then bounded small alignment/rate,
then explicitly aggressive spectral+attack+decay-envelope similarity. Fornoise
percussion, known sharedROM andnative rates are preferabletopitchestimates;
unknownpitch mustnotbe reportedas complyingwith100cents. Matchingfrequencyalone
is not enough to mergedifferentdrums. Judge actualdecoded ADPCM withrealprofile
quantizedgain/rate ineachplaybackgate; keep sourceonset/endunchanged andcheck
both missingtail andextratail for everygroupmember.

Useexisting representative(medoid) ratherthanwaveformaveraging. Firstcheck all
members'onsetgain+fitratio canbe realizedwithoutclipping,upperlimit orminimum
nonzerosilenceerrors. Prefer strongerrepresentative when needed; splitgroups
ifno feasiblechoice. Held envelopes mergeonlywhen theirshapes are compatible;
ordinaryheldPCM@v cannotreconstruct differentcurves. No duplicatebakedgain.

Nochainclustering. Eachmembermustmatchrepresentativeandgroupwidepitchspread
mustmeetpolicy. Rankbyerror,usage,bytesandrestoredomittedplaybacks;preserverare
butdifferentinstruments. Do notclaim96slotsornativebufferfitinadvance.

Proposeddiagnostics: original->group/representative, gain/cents/rate/lag,
waveform+spectral+envelopeerrors, effectivegainquantization,tailchecks,original/
representativedurations, candidate->representative->retainedcountandbytes,
newlyrecoveredplaybacks,groupA/BWAVsequenceandworstcases. Numericalthresholds
remainexperimental; user listening determines practicality.

Architect reviewed: finalplaybackconditions,feasiblegainfirst,unknownnoise
pitch,allgate-tailchecks,andactualfinalallocationrecoveryaremandatory. No
implementationwasrequested inthisturn. GFII04/06andPSGworkremain deferred.
