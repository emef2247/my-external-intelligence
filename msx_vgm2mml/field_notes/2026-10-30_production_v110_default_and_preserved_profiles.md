# Production v1.1.0 baseline and preserved comparison routes — 2026-10-30

## Explicit user decision

Restore v1.1.0-equivalent ordinary production MML as the default. Preserve the
current enhanced pipeline for comparison. Also retain the present legacy-macros
and legacy-loops behaviors under clearer alternative names. User will run the
local regression; validation here is intentionally small because quota is low.
This supersedes the interim decision to default export to hybrid legacy macros.

## Implementation

export_mgs defaults: --macro-mode v110 --loop-mode v110. Isolated production_v110
modules come from tag v1.1.0 (only sibling imports are package-qualified):
mml_macros, structured_macros, mml_sync, mml_envelopes, pre_envelope_loops.
The historical enhanced allocator was already the v1.1.0 default, so restoring
that policy requires both its base compressor and its enhanced allocator, rather
than setting the legacy switch. Historical envelope/loop rendering is selected
for the default non-score-normalized, non-guided production route.

Current relative/phrase preprocessing remains unchanged and is not deleted.
Production macro dispatch is scoped with a ContextVar after reference/blueprint
creation; the baseline does not rewrite the musical layer or native evidence.
Final ordinary notation compaction is skipped in v110 macro mode. Current layout
is retained; MML whitespace can differ even when compiled bytes match.

Preserved comparison selections:
- --macro-mode current / --current-macros (also --enhance-macros): current
  enhanced allocation and shared relative/phrase preprocessing.
- --macro-mode greedy / --greedy-macros: current shared preprocessing with
  greedy allocation, formerly --legacy-macros.
- --loop-mode current: current normal loop-first rendering.
- --loop-mode envelope-first / --envelope-first-loops: prior --legacy-loops.
- --loop-mode score-guided / --score-guided-loops: current score-guided route.
Old legacy spellings remain hidden compatibility aliases. Both axes can be
selected independently. Previous enhanced default = current/current; previous
hybrid legacy+legacy = greedy/envelope-first. Blueprint retains current policy.
Explicit --score-normalize uses the current timing-aware envelope renderer and
is outside strict historical reproduction. Canonical/batch defaults remain
current for existing callers; export explicitly forwards profiles into batch
commands and records them in effective diagnostics. README unchanged.

Validation: 12 export tests passed, including real public conversion, reference
and blueprint/MIDI publication, defaults, aliases and CLI forwarding. A single
SALAMD01 historical comparison is being checked separately; no broad local
regression. Docs/production_profiles.md describes policies. No commit/push was
requested for this task; changes remain in the working tree alongside the
preceding HuC loop recovery changes.

## Completed single-input baseline verification

SALAMD01 default export + force + skip-keyon-counts succeeded. Compiled MGS is
byte-identical to saved test2/v1.1.0: 11,264 bytes, SHA256
3850165e2e00294e2c24a73385de15678d03adee2efb46579e56b0e870d1e755.
MML is 12,281 versus 12,373 bytes; whitespace-insensitive equality: False.
No listening claim and no broader local regression. Evidence: ignored
outputs/diagnostics/v110_restore_20261030/comparison.json and export diagnostics.

Additional limit: MML differs even after removing whitespace/comments, although
compiled MGS bytes match exactly. Do not attribute every textual difference
solely to formatting or claim byte-identical MML restoration. Broader tests
remain with the user.
