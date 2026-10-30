# MGS definition terminator and silent successful compilations (2026-10-24)

The user reported silent ordinary export_mgs public OPLL MML, independent of
--enhance-drums. Available tools: native MGSC 1.11 and libkss-js actual playback;
compiler source revision is unavailable. Do not promote body estimates to all
MGSC versions or claim browser GUI listening was performed.

## Confirmed reproduction and repair

The broken ordinary/default and legacy OPLL exports compiled but produced zero
melodic Key-Ons. Handwritten music at both 15000 and16000 allocation totals
played; raising the global budget was not the explanation. Controlled generated
setup allocation84->85, reducing a music share by1 within total16000, restored
6 Key-Ons. Packed MGS size stayed384bytes; only byte152 changed2c->ff. Separate
PSG tune25->26, envelope5->6 and uservoice10->11 tests reproduce the missing
terminator. SCC/combined bodies can already have conservative slack.

py/mml_sync.py::definition_allocation reserves one extra byte once per nonempty
setup stream. Empty definition allocation stays0. Native source timing, Segments,
normalization and musical rendering are unchanged. Git blame places the former
body-only estimate in b5027feb (2026-10-06), Include definition storage in the
MGSC allocation budget. This is a shared allocation bug, not drum inference.

## Verification and limits

Public default OPLL19/19 compile and actually play nonzeroPCM; melodic channel
Key-On counts match, paired nearest-note pitch labels match, and rhythm-instrument
rising-edge counts match. These are separate checks, not audio equivalence.
custom_voice retains up to283.605ms raw onset drift near43s under inferred130BPM
normalization. Instantaneous zero-duration edge snapshots do not establish audible
volume errors. PSG/SCC native tick comparison uses explicit actual-minus-reference
startup offset2ticks and boundary tolerance1tick; activity/boundary differences
remain visible. Full public matrix and exact counts are in the saved verification
report when complete. Public compile buffer failures must remain reported.

New tests/test_mgs_playback.py checks actual compilation/replay for every setup
class, the broken successful-compile/zero-Key-On reproduction, and default public
export+PCM. Optional unavailable runtimes skip explicitly. batch keyon_status now
says mismatch when melodic counts differ, and export exits1. A PSG/SCC-only0/0
comparison still is not proof of playback. scripts/probe_mgs_audio.mjs measures
actual rendered PCM separately.

Reports: msx_vgm2mml/outputs/diagnostics/mgs_silence_20261024/verification.md/json.
Large pass dumps and probe binaries remain under Windows TEMP/msx_silence_20261024.
Previously generated MML must be regenerated. No staging/commit/push by this task;
existing staged frontend/performance work and unrelated private notes are retained.

## Final public matrix

60 inputs complete conversion attempts:56 compiled and nonzero5s PCM,55 complete
melodic count comparisons,4 Track buffer full (cpebach/enchanted/turpinh/bridge).
bluemtns remains export-duration guard error104.67s; extended240s still reaches
cap,269/269 melodic attacks. OPLL last write80.15s, MML tracks77.56–82.06s, SCC
zero-volume envelope writes continue. No guard suppression. All26 standalone
PSG/SCC have zero pitch/coverage differences,3 have activity differences; explicit
startup offset2ticks/tolerance1. Mixed giselle has140 pitch-difference ticks;
other mixed activity/boundary differences remain.35 focused tests pass. Final
report and regenerated public artifacts are saved at the path above.
