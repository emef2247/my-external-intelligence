# Score-only blueprint and independent source products

## User intent and superseded policy

Blueprint is an editable structured score, not a timbre-faithful arrangement.
The latest request supersedes earlier distinct placeholder @ numbers. Remove
ALL executable instrument selections and volume commands, patch/wave definitions
and SW envelopes. Keep source chip/instance/section/channel and synthesis
parameters/waveforms as documentary comments and sidecars. Do not strip these
curves from NativePerformance. PCM remains excluded.

A complete gate may contain repeated pitch phrases. Compare score atoms
(pitch, duration, rest, attack-or-hold), not whole gates or source voice IDs.
SourceLoopPlan verifies exact expanded commands, then existing macros compact
text. A source timbre change cannot consume score channels or split a note.
Keep source unit IDs and score-atom ranges so future envelope assignment can
attach different curves to equal score skeleton occurrences. That later
reattachment is not implemented by this change.

## Implementation boundaries and review

Native source construction/dumps precede the entire accepted MGS execution.
A valid native layer survives failed target timing/policy lowering. Preserve
accepted legacy execution on success and report a failed projection explicitly.
Default canonical and MGS export output includes reconstructed reference VGM.
Each artifact has independent ownership/status. Requested failed VGM plus valid
MML produces aggregate partial_failure/nonzero, but batch still compiles the
explicitly successful current MML. Never label old retained products as new.

Blueprint uses tempo75 (one target step=1/60 second) and a shared monotone map.
Positive rendered note/rest/semitone intervals require at least one legal step;
independent nearby timestamps need not be serialized. Dense source events can
expand duration without a universal error bound. Record displacement/expansion;
never call this source-native timing. Overlaps/capacity overflow use additional
parts/pages. Pitch unknown/folding is explicit. Terminal OPLL rhythm attacks
receive a documented target-only tail shared by every page. Stops are mapped
through the same clock without adding score-duration constraints.

Non-OPLL percussion remains an explicit blueprint limitation: event IDs, chip,
family and reason appear in comments/excluded_percussion. Do not silently call
OPNA rhythm PCM-excluded or claim complete source score mapping.

Loop layout is whitespace-only: newline at opening bracket, nested indentation
after channel prefix, no forced newline at closing bracket. Preserve canonical
ordinary MML CP932 encoding. A real Japanese SALAMD end-to-end run exposed an
incorrect UTF-8 read in the new final formatter; fixed CP932 and covered by a
non-ASCII title test. Native MGSC diagnostics must receive the original encoding;
writing an otherwise equal Japanese test copy as UTF-8 creates binary differences.

## Verification, limits and restart

206 related authored/integration checks pass with no failures/errors/skips.
Fresh default SALAMD01 batch export produces ordinary MML/MGS, VGM and blueprint.
Old/new blueprint score-line characters20816->11937, finite-loop openers44->102;
total text26304->18008. Old output DID contain loops. These are textual measures,
not executed repeat counts, MGSC buffer savings or listening verification.

Exactly the previously authorized27 local inputs were rerendered from saved
native music in isolated diagnostics:27/27 blueprint texts generated,32pages,
31 MGSC compilations. Only contrail first page has track buffer overflow;
all14 SALAMD blueprint scores compile. Normal whitespace verification26 texts
preserves commands and is idempotent;24 successful MGS binaries are identical,
2 ordinary SALAMD02/06 buffer failures unchanged. Inputs/existing publications
were hash-checked unchanged. Native control lookup was indexed by device,
section/channel and binary-searched time instead of scanning all controls per
unit, preserving provenance order and enabling dense contrail analysis.

No hardware listening or automatic envelope reattachment was performed.
Evidence: public-project ignored outputs/diagnostics/blueprint_v2_20261027/.
README remains user-maintained and unchanged. No staging/commit/push in either
repository. Preserve unrelated private notes/user scripts/logs. Current branch
codex/musical_HAL; next is user review, HuC/KATOKEN work deferred.
