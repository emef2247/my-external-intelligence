# v1.1.0 parity history and restoration proposal — 2026-10-30

## Confirmed historical parity

The user's recollection is correct: introducing the musical layer did not
itself cause the currently observed ordinary-MML differences.

- 2026-10-26: fixed main ab58c3e versus HAL implementation 9da7011 matched
  29/29 ordinary MML and 29/29 compiled MGS, including the long SALAMD01.
  See 2026-10-26_main_hal_local_mml_parity.md.
- 2026-10-27: the user's completed run was compared with test2/v1.1.0.
  Among TWINBEE7, SALAMD14, FMPAC5 and contrail1, all 26 emitted ordinary
  MML and 24 compiled MGS were byte-identical. SALAMD02/06 failed compilation
  in both; contrail failed before MML in both. Input hashes and FM24ms /
  normalization8ms agreed. See 2026-10-27_v110_user_run_parity.md.
- Later blueprint/ordinary layout verification retained the same 24 compiled
  binaries; formatting alone did not establish the later capacity regression.

## Why current legacy + legacy is not a v1.1.0 preset

Direct inspection of the v1.1.0 tag and current code confirms:

1. v1.1.0 already defaulted enhance_macros=True. Its --legacy-macros selected
   the earlier greedy search; its default was not legacy + legacy.
2. v1.1.0 already had --legacy-loops. Its ordinary default selected
   LoopFirstEnvelopeBank, while that option selected EnvelopeBank. Thus the
   option names refer to earlier algorithm generations, not a release preset.
3. Current py/mml_macros.py calls prepare_phrases(normalize(text)) before
   the legacy compressor. v1.1.0's compressor did not have those passes.
   Relative octave/volume preparation and complete-phrase/adjacent-repeat
   selection were added after parity, on 2026-10-28, to shared production and
   blueprint processing. Skipping the late enhanced allocator does not skip
   the shared preprocessing.
4. Enhanced allocation also changed from total-text ranking to macro-count
   priority and phrase-class constraints. This was explicitly requested then,
   but fewer macro definitions did not guarantee smaller compiled tracks.

The statement that canonical/batch defaults were unchanged only described
where the latest frontend default change was applied. It did not mean their
underlying shared implementation still equalled v1.1.0.

The six-way same-code A/B supports an extra late-enhancer capacity effect:
08/11 fail in every enhanced mode and recover in every legacy mode. However,
which shared pass causes each remaining cross-version byte difference has
NOT been isolated. Do not state that blueprint, relative notation, or musical
layer semantics were proven incorrect. Target-text-preserving transformations
can change opcode cost and macro/loop choices without corrupting the score.
Macro-count/text objectives and unchanged #alloc after late rewriting are
credible capacity concerns, not a proven per-command cause.

## Latest user direction and assessment (proposal, not implemented)

The user wants a v1.1.0-equivalent default production path. They question the
value of retaining today's hybrid legacy-macros route, and suggest preserving
the older loop algorithm under a distinct name for later reevaluation.

This is a sound baseline strategy: retain the musical layer and independently
useful reference VGM/blueprint/MIDI, while restoring the verified v1.1.0 ordinary
rendering/compaction behavior as a coherent policy. Do not revert the architecture
or blindly combine two legacy switches. Isolate post-parity preprocessing from
that production baseline, then evaluate enhancements one at a time against it.
Blueprint can retain its own score-oriented formatting/compaction policy.

Today's hybrid legacy-macros need not remain a permanent public choice, but keep
it available internally until the baseline replacement is verified. Likewise,
older envelope-first loops may be an explicitly named experimental alternative;
final CLI naming/default changes require implementation work, not this note.
Current legacy+guided had a small aggregate MGS advantage over v1.1.0, so retain
its measurements as a future candidate rather than claiming it has no value.

No converter code, options, defaults, generated regression products or README
were changed during this record-only task. No conversion/tests/commit/push.
