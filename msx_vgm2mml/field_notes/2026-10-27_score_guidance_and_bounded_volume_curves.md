# Source score guidance and bounded volume-curve prototype

## User direction

Blueprint's score skeleton should guide production; the generated MML and its
coarse clock must not be parsed back. Repeats apply to all target chips, while
SW-envelope selection is a later PSG/SCC-specific operation. Autonomous HW
phase is not score identity or a repeat barrier. Preserve actual reset and
reconfiguration operations; never add phase-restoring resets at loop entry.

## Implementation and review corrections

score_structure builds native-time score events before accepted target lowering.
Blueprint consumes those pitch spans. Production maps whole accepted notes
many-to-many, abstaining on gaps, overlaps or multiple source units. No guide
cut creates attacks/restarts. Opt-in --score-guided-loops covers PSG/SCC and
ordinary/normalized OPLL, using exact command-expansion equality and the prior
plan as fallback. Other native melodic chips have source guides but no new
MGS mappings. For a score-only motif retain every exact settings/curve variant
at each structural leaf slot before bank selection; first-occurrence counts
or global curve deduplication are invalid. Shorter text is not compiled cost.

Independent default-off --psg-state-ms/--scc-state-ms require explicit maximum
and durationweightedmean absolute volume-register error budgets. Units0..15
are register levels, not acoustic amplitude, dB or percentage. Reuse only
simpler observed compatible equal-duration curves; compare original directly,
not transitive clusters. Native sample and renderer-frame checks protect
endpoints, audibility, point/absolute integrated/mean error and longest changed
interval. Preserve notes, pitch, wave, native controls and original Segments.
Noise/HW/morphing/unknown-time exclusions apply ONLY to volume approximation.
Representative source channel/note/Segment/gate/sample IDs are recorded.

A later normalized clock can clip unchanged tails and invalidate a budget.
Until post-lowering comparison exists, applied normalization causes explicit
abstention/original curves retained, with candidate decisions preserved. A
mere normalization request without accepted clock does not block approximation.
Backend partition fallback slices the accepted curve across old partitions,
never silently restores original values. Authored combined-option test covers
abstention. Search cap64 and deterministic ordering are explicit.

## Validation and limits

322 related tests pass. Actual MGSC/libkss checks cover onset0/1000samples and
full-bank inline fallback. Settled frames show intended difference1level for
oneframe, otherchannelunchanged. Raw instruction dispatch jitter1sample causes
instantaneous peak5 at a steep step; record separately, never claim samplewise
or acoustic equivalence. Existing native-to-driver quantization is independent.

Frozen-code fourmode comparison uses only previously authorized local inputs.
SALAMD02: MML23387->23063bytes, still buffer-fail on PSGtrack1 in allmodes.
SALAMD06:21338->21318guide-only->20492with24/48ms; still buffer-fail on PSGtrack2.
24ms replaces4PSG+5SCC notes; SCCcurves92->91, PSG24unchanged. GRA2split01:
3674->3628MMLbytes; allcompile, MGS1536bytes acrossmodes (different binary,
not a size gain). No additional48ms gain. This does not reproduce OPM's large
short-state reduction: pitch/detune/noise/HW controls remain. Do not claim
compile fixes from MML text savings. Latest workingcode SALAMD06/GRA2 24ms MML
matches frozen grid byte-for-byte after normalization/OPLL additions.

Evidence under ignored outputs/diagnostics/score_guided_curves_20261027/:
analysis_ja.md, comparison_final, current_verification.json, integration.json,
frozen_runtime/code_sha256.json, authored playback and HW probes. Source hashes
unchanged, existing user products untouched. No private fixture contents belong
in public docs or commits; only aggregate findings retained here.

## HW target articulation finding

Actual MGSDRV/libkss sends repeated shape writes at successive notes even when
MML selects the shape once (first2seconds of authored probe observed). q0 alone
does not prevent it; changing-pitch ties reduce but do not uniformly remove
resets in that probe. This is target articulation, not a justification to put
HW phase in score equality. Do not claim autonomous source HW continuity is
repaired by guidance, or blindly remove shape commands. Native reference VGM
single-source-shape10 write is retained once by a regression check.

## Restart

Experimental switches remain default-off; native ScoreGuide artifacts precede
target lowering by default. Export FM24/normalization8 unchanged. README/user
scripts/logs untouched. No stage/commit/push in public or private repo; preserve
unrelated notes. Current codex/musical_HAL. Next user listening/sweep review.
Pitch/LFO approximation, relative envelope families, arbitrary envelope
reattachment and full HW-legato repair are future work. Do not resume HuC or
port incomplete target rules into other repositories without user direction.
