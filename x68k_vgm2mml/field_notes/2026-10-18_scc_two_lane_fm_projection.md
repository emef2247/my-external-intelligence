# SCC musical units and two-lane OPM fitting (2026-10-18)

## Request and scope

User authorized SCC->FM for x68k and MUCOM, permitting the easier first target.
MSX SCC musical interpretation, SW envelopes and structure must be reused.
x68k is the implemented first target. User performs private/native regression.
No Git stage/commit/push/branch operations. M_G2_01S.vgm was left untouched.

## Interpretation and target design

New source-sample adapter uses the inherited MSX mml_envelopes.extract_notes
with chip=scc on immutable replaced views. Source positive durations remain
original sample intervals; last observed state extends only to known VGM EOF.
Nonincreasing same-setting volume runs remain one gate; source pitch writes,
wave/enable/settings changes and rising volume infer boundaries. Zero-time pitch
writes carry their boundary to the next positive interval. SCC has no native
OPM Key, so every musical target attack is labeled inferred.

Source .scc.music.csv links notes/runs to original rows and waveform. Shared
source_plan/SourceLoopPlan builds source-unit audit structures. Final MML still
consumes the ordinary generated OPM VGM -> native projected analysis -> musical
units/control runs/structure path; its shared solver is MSX-derived. This does
not import the whole MGSDRV renderer or its @e/32-bank/220-byte restrictions.

Default additive retains four bins1..15. additive8 optionally adds next four.
FM candidates are offline voice banks; importer never invents a generic SCC FM
patch. Primary SCC/PSG/noise lanes reserved across the complete rendered input
including replay. Nonoscillating but initialized SCC lanes also reserved.
Extras ranked by duration*(volume/15)^2*improvement. No ch time-sharing or source
omission to obtain quality. Two-part fit without a spare falls back to original
one-part additive; fully silent fitted parts are removed before budget tests.

The physical target lane is now part of performance write/init/terminal grouping
and gate state. Both lanes use common source note IDs. Modulator TL stays fixed
under software-volume changes; only carrier TL changes. Wave transitions retain
full explicit algorithm/feedback/MUL/TL parameters. TL bounds/mix saturation,
oscillator phase and unselected/DC/Nyquist16 content remain approximations.

## Offline measurement

Authored wrapper loads external .tools/mdxtools YM2151; no core vendoring.
Requires WSL GCC/NumPy. Common TL0 carrier calibration, gain and audio scale;
no individual waveform/lane normalization. Actual simultaneous A+B rendering
scores Hann FFT magnitude error, including excess components/gain. Bounded
algorithm0/4/5/6, feedback0/3, MUL1/2/3/5/7, modTL24/40/56 + local refinement.
Models: additive, additive8, additive8-lowhigh, FM+additive, FM+FM.
Reference is zero-mean32-step held table at quantized nominal target frequency,
not an independent SCC emulator. Fixed voice checks at half/ref/double pitch
are limited transfer measurements, not all-range quality proof.

Bank keyed by full wave SHA256, with chip/clock, reference gain/pitch, search
version, core/header/wrapper hashes, measured sample rate/frames and calibration.
Loader validates these shapes and parameters, without requiring a local matching
core. Invalid/missing selected waveform/model stops. Worse FM reference score
falls back explicitly. Import scores do not establish benefits at other pitches.

## Saved-before-check evidence

20 focused checks passed: 4 SCC source/lane/allocation/bank, 2 external actual
core fit/cache, 5 existing performance, 9 existing PSG/SCC target checks. Tests
used only public/authored data. Existing compact default artifact list retained;
new standalone voice_projection report is emitted for nondefault SCC models.

Public tests/fixtures/public/scc/volume_sweep/volume_sweep.vgm:
- additive8 and FM+additive canonical --dump-passes generated source notes,
  source structures, physical lane/Key/TL writes and structured MML.
- Eight descending volume runs of the initial sounding note remain one unit.
- Both lanes have inferred attacks at samples779,47850,53736,59622,65508,71394,
  77280,83166. Source rests and all original Segments retained.
- FM AL4 modulator TL32/24 unchanged; carriers32,32 atvolume15 ->34,34 at13
  ->36,36 at11. Bank reference440.020597Hz errors: additive .3146864007,
  FM+additive .1711754257. No claim of native sound equivalence.
- Both export_mdx listening outputs with --compiler mmlx --no-song-loop --no-vgm
  succeeded. Additive8 selected16640us/tick. No native listening here.

Artifacts: outputs/diagnostics/scc_projection_20261018/additive8, fitted_bank,
fm_additive, listen, listen_fm. Authored sine/square comparison from independent
fitter remains outputs/diagnostics/scc_fm_fit_authored_20261018. FM does not
universally beat additive; for authored square reference scores were additive
.0509, additive8 .0282, FM+additive .0399, FM+FM .0608. Listening still required.

Review caught two issues before completion: silent fit part consumed a spare;
provenance fields were not required. Fixed and checked; focused rereview clear.
Existing target test artifact assertion initially failed because standalone SCC
report was added by default. Fixed contract rather than changing test. Exact
sample-boundary guard message aligned with existing expected wording, no guard
relaxed. No private fixture contents were copied or converted.

## Next

User listening/regression of opt-in additive8, then fitted bank. MUCOM adapter
still pending: reuse source musical units and structure, but define OPNA6 budget
and YM2608 probe/candidates. Do not treat OPM bank as validated for OPNA.
Duration windows and optional VGM output remain paused. No broader PCM work.
Public detailed specs live docs/scc_fm_projection.md and docs/scc_fm_fit.md;
project_knowledge Final Rule left unchanged.
