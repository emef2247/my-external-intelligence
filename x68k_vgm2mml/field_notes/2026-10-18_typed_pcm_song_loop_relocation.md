# Typed PCM FM song-loop relocation (2026-10-18)

User authorized loop assembly fix only after PSG-PCM full-song regression.
SW envelope/track-size optimization deferred. No private conversion/compiler or
native listening reruns. Existing GRA2_05/08/12/14/15 fail outer-boundary check;
prior SegaPCM notes also report the same helper error. Their new success is not
claimed until user regression.

Pinned local mmlx0.2.0 compile.rs:245-270 uses loop_adjustment=1 rather than3
when source L begins with Note/Rest and later finite repeat exists. soundlog0.15
F1 interpretation is relative to command end. Therefore compiled target is two
bytes after L, possibly an operand OR a different valid command boundary.
Reject-only-invalid-boundaries or relaxing the guard would miss/corrupt cases.

Scoped compile_pcm_fm adapter parses original AST, compiles full FM as before,
compiles per-channel prefix before one outer L with same state, removes implicit
finite End and requires prefix command equality. Parsed repeated channel lines
are merged by mmlx. Source byte target determines correct signed16 negative F1.
Repair only the verified +2 discrepancy with same duration/later-repeat trigger;
other discrepancies reject. Original source MML/IR, duration/control commands,
finite repeats and correct offsets unchanged. Relocation track/sourcebyte and
old/newoffset written to stderr retained as .pcm/mdx_build.log. Existing strict
outerboundary/positive loop/sharedFMPCM ticks and end/same serializedFM guards
remain. Generic FM-only compiler is untouched.

Reject nested/multiple/unreachable L and cross-L portamento changing prefix.
Compiler/body/native offset limits still fail normally; no fallback loop removal.
Generated end/loop periods preserved, no nearest-neighbor offset guess.

Saved source before tests. Nine focused direct_pcm Rust tests pass (3new groups,
6existing). Authored duration-first note+rest reproductions check malformed and
valid-but-wrong target; multiline L inherits o2/l8 and emits expected21+128 note
for24ticks. Test portamento c%2 L _ d%3 rejects changedprefix. Full serialized
16track FM A/B + typedPCM P has L2 and EOF9, concrete F1 targets at commandstarts.
These are boundary/timing checks, not two-pass PCM runtime/acoustic validation.
Early integration-test assertion incorrectly passed E8 to FM-only timing guard;
test now strips the assembly marker. Added pitch expectation initially assumed
wrong octave convention; corrected to actual mmlx formula octave*12+base-3.
No production guard relaxed for those test failures. Release helper rebuilt.

Architect plan/final reviews found no blocker; recommendations added source
context/known-adjustment guards and inherited/default/portamento assertions.
README/other dirty user changes preserved. No stage/commit/push/branch mutations.
Next user runs --force regression; capacity/SW envelope work stays deferred.

Release CLI authored reproduction saved at outputs/diagnostics/pcm_song_loop_20261018.
PDX build and typed MDX build succeeded; command census inspected directly.
E8-expanded A F1 target byte4 Note, B byte1 Rest, P byte1 Rest. Source L log A3/B1
is before layoutE8; relative offsets remain correct after insertion. No listening
reference or PCM replay certification is claimed. Final 9focusedtests pass, both
public repository diffchecks clean; unrelated existing dirty files preserved.
