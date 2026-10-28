# 2026-10-28: relative octave/volume preparation and macro-frame priority

User requested this as mandatory ordinary-MML preparation, with macro-slot
reduction taking priority over text length. Inside macros, all octave/volume
changes must remain relative, even when longer than absolute notation. Only
callers may shorten three or more relative steps to oN/vN. The preceding
blueprint/HuC changes had already been committed by the user.

Implemented mml_relative_state: expand generated definitions, retain entry
setters, remove redundant setters, and express internal changes relatively.
Selection rejects absolute octave/volume setters inside bodies, including
nested loops. Rhythm setters stay outside rhythm macros. Loop bodies start
with unknown state so necessary per-iteration resets survive. Finite transfer
tables preserve nested-loop exits without exponentially expanding repeats;
out-of-range/unknown state is treated conservatively. Source evidence is intact.

Default structured search compares macro count first, then complete text length.
This bounded portfolio is not a globally optimal allocator and can increase
text when entry resets must remain outside definitions. Baseline compression
and blueprint use the same preparation. Pre-existing external definitions
passed directly to the legacy compressor retain its historical untouched rule.

Dump-passes now exposes macro_state.before.mml, macro_state.relative.mml and
macro_state.json (absolute/previous/emitted state and target-tree locations).
Equivalence checks compare timed commands with effective octave/volume and
final state, rather than insisting on identical setter spelling.

73 focused tests passed: shifted octave/volume sharing, nested resets/exits,
unknown/range guards, physically wrapped loops, rhythm separation, normal
formatting/blueprint and MGSC/libkss playback. Authored OPLL register values/order
match with <=4 source-sample dispatch jitter. Different absolute/relative
bytecodes are not expected to produce identical binaries or sample-exact audio.

Supplied GENPEI01 blueprint reprocessed into a separate ignored directory:
32 macro definitions ->12; characters13046 ->13032. Effective timed commands
and exits match. Both original and transformed MML compile with native MGSC.
Original user product was not overwritten. Local evidence/passes/compiler logs:
outputs/diagnostics/relative_macros_20261028. No full private VGM regression.

README/user scripts/logs were preserved. Changes remain uncommitted; no push.
MUCOM musical-layer work remains paused. Next: user regression/audition.
