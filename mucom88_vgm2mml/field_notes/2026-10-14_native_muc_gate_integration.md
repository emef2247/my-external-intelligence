# 2026-10-14 Native musical MUC connection after reset

User authorized proceeding from copied x68k baseline, prioritizing YM2608 VGM ->
compilable MUC/MUB. Source immutable gates own internal control trajectories;
shared exact source_plan extracted unchanged and reused (12 existing MDX music
checks pass). Default native MUCOM route connected to separate FM/aux/timing
projection, CSV/JSON loops/units and isolated compiler adapter. Archived batch
ownership/hash protection reused; old Segment renderer not restored.

Native profile is local MUCOM88 Windows 260407 mucomDotNET MUB8, 7987200 Hz.
Native H changes operator AM bits; restore yDR after initial, held and released
H/p edits. Actual MUC/MUB/native VGM probe established this ordering, not DAT
identity alone. All source TL restored explicitly after v15. Held tone edits
allocate no unloaded DAT slots; 300 held profiles regression passes.

Reviewer found two silent-loss paths: zero-time/transient-partial FM key edges
excluded from positive gates, and trigger-then-same-time rhythm controls retaining
stale prefix. Added ordered source edge audit/rejections, rhythm post-trigger
control/zero-span rejection and regression source VGM tests. Nonloop EOF-held
FM has explicit close diagnostic; held loop-end gates reject unsafe retrigger.
Native verifier now compares positive normalized control interval counts and
boundaries, not only midpoint values, plus source/native ordered keys and
operator/pitch/pan/LFO. Third loop boundary is cropped before native edge audit.

Final outputs: outputs/mucom88/integration_verified, 26 MUC/MUB compilations,
20 FM checks passed. FM looped patterns checked two traversals. 61 focused
Windows tests passed; subsequent default MUCOM CLI check added and exercised
in WSL. WSL original from_fm batch 16 successes, PCM fixture native bank/MUB
check included. Compatibility 71 cases:70 pass plus inherited normalization flag
failure. Full reset suite failures remain inherited; no green broad-suite claim.

Limits explicitly retained: collapsed internal slices, released-envelope controls,
SSG/rhythm/PCM equivalence, decoder waveforms and listening not certified by FM
checker. PCM output preserves original encoded bytes and bank binding; no PDX
constraints. Source keys/evidence, timing boundaries/errors and gate trajectories
are inspectable. User handles listening/commits. No stage, commit or push in
public/private repositories; unrelated private notes preserved.
