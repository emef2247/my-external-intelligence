# 2026-10-29: HuC supplementation withdrawn; independent blueprint timing

The user accepted the existing BIKKR projection and explicitly withdrew the
new OPLL/PSG supplementation. Restore committed HuC behavior; mapping losses
are acceptable target constraints. Future user-authored CSV source/target
mapping is an idea, not authorized implementation scope. Do not interpret this
as a fixed four-channel limit for all HuC inputs.

Direct BIKKR03 source inspection corrected an earlier hypothesis: no enabled
waveform RAM writes were observed. Native loop validation detected differing
second-traversal states on zero-based channels1 and4. The existing force subset
retains0,2,3,5. This was deliberate exclusion after repeated-state analysis,
not failure to analyze the second loop. Source/native reference stays complete.

The discarded experiment mapped all6 via4SCC+2OPLL or2PSG and made the target
finite while retaining the original source loop in the reference. It generated
MML but full BIKKR MGS exceeded the fixed16000-byte allocation pool. Per-track
compiler probes also exceeded that total. An authored playback check still
failed its SCC activity assertion; do not claim six-voice playback recovery.
All experimental HuC code and its new switch/tests were removed at the user's
request; ignored generated outputs remain historical, superseded evidence.

Separately retained --blueprint-score-normalize derives a renderer-only score
view from original native samples at --score-tempo(default225). It leaves the
production plan/reference/guide untouched. The guide is an in-memory source
score representation, never parsed from exported blueprint text. Authored
integration comparison verified byte-identical production MML/MGS/reference
VGM and equal guide JSON with blueprint-only timing on versus off, while the
blueprint changed. --score-guided-loops remains off by default. README unchanged.
