# Inferred OPLL percussion augmentation — 2026-10-24

User requested inferred drums with added OPLL rhythm after the performance-IR
milestone. Implemented opt-in --enhance-drums across converter/export/batch.
No changes to native source interpretation, PSG/SCC rendering or source fixtures.
No staging, commit or push.

py/drum_inference.py catalogs hypotheses and abstentions from PerformanceIR.
Gestures begin at audibility, strong volume rise, effective EG reset or entry to
noise; pitch writes alone stay internal. Same-sample setup is retained. PSG noise
up to80/220/350ms suggestsHH/SD/CYM. Short monotonic downward pitch sweeps of>=3
semitones plus>=3-level fixed-volume decay suggestBD/TOM. SCC can also qualify
with duration-weighted broadband32-sample DFT features plus decay; stable decaying
pitched notes abstain. Hardware EG register levels are not measured amplitude
and cannot establish tonal decay. Target level is a2/3 peak-register proxy,
not calibrated loudness. Unknown sample timing rejects classification.

py/drum_enhancement.py keeps source parts and merges optional target Hit/Group
objects, never fake native Segments. Native same-instrument/time hits win;
inferred duplicates use stronger level and deterministic tie-break. Native
high OPLL channels6..8 Key-On/frequency, even muted/zero-length, block additions.
Mode1 is selected only after at least one additional hit is projected. No hit:
original native mode/target bytes remain. --force does not override this.

Normalization uses actual PSG/SCC projected target boundary correspondence and
bounds source-time error. Main snapshots pre-normalization control text in memory.
Review found a P1 in the first draft: required before.normalize files were saved
only with --dump-passes, causing ordinary applied-clock runs to reject all hits.
That dependency was removed. A real applied-clock authored test is rerun in the
same outdir dump-on -> dump-off: identical MML,6 hits, no stale before files.
Native rhythm keeps its own retiming/collision policy; all merged gaps and final
minimum length are recalculated. Oversmall/unmatched/unsafe additions are logged.

Validation:78 related checks pass; after the P1 fix,10 integration checks pass.
Public28 compile and preserve original PSG/SCC effective timelines;25 no-hit
outputs match earlier MML bytes exactly. Three existing rhythm-suite allocation
assertions still expect15000 rather than committed16000;4 raw/ordinary native
rhythm render comparisons match HEAD bytes. This is not a new rhythm regression.

Representatives from the prior explicitly authorized59-input set:
- DSLY4_01:5 additionalBD hits; source SHA/timeline retained; MGS compiles.
- gra2_001_with_sync_mark01:SD7,HH3; source SHA/timeline retained; MGS compiles.
An authored three-noise-burst VGM also compiles through the actual export entry.
No full private catalog rerun or auditory classification-accuracy claim.

Small listening examples + reports copied to repository-local ignored outputs:
outputs/diagnostics/drum_enhancement_20261024/.
Public dumps/logs remain at C:/Users/ef110/AppData/Local/Temp/
msx_drum_enhancement_20261024. Candidate/projection CSVs are the next refinement
input; preserve the experimental status and original music while listening.
