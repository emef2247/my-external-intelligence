# 2026-10-15 Project knowledge slimming (documentation only)

User requested preserving all principles, specifications, constraints and open
conditions while reducing the core document to roughly500–700 lines. No code,
tests, README or AGENTS change; no private conversion/replay/listening, staging,
commit or push. Source snapshot is retained locally in ignored
outputs/diagnostics/project_knowledge_slimming/project_knowledge.before.md.

## Chapter destinations

The table records semantic coverage, not a claim that every old paragraph was
copied. Existing focused specifications were compared first; only absent facts
were added. Details and their public rationale remain available without these
private notes. No blanket public archive or proliferation of new topic files.

| Former chapter | Retained/merged destination (docs/) |
|---|---|
| 1. Project Goal | project_knowledge.md §1; asset ownership preserved |
| 2. Canonical Frontend | project_knowledge.md §2; canonical frontend and explicit target selection |
| 3. Primary Rule: Intermediate Results Are First-Class Artifacts | project_knowledge.md §3 unchanged |
| 4. Transformation Boundaries | project_knowledge.md §4; source, sounding units, structure, target boundaries |
| 5. VGM Is a Register Event Stream, Not a Score | project_knowledge.md §5 unchanged |
| 6. Preserve Evidence and Derived Meaning Separately | project_knowledge.md §6; unknown, assumed and inferred fields |
| 7. Timing Is Source Evidence | project_knowledge.md §7 unchanged |
| 8. Key Edges, Retrigger, Continuity, and Zero-Length Events | project_knowledge.md §8 unchanged |
| 9. Chip-Specific Durable Semantics | project_knowledge.md §9; opna_segments.md / opm_segments.md / psg_scc_segments.md / segment_intermediate_format.md; only missing OPLL/waveform details added |
| 10. Rhythm Semantic Layer | project_knowledge.md §10; existing segment_intermediate_format.md §§6–9 and MUCOM action projection |
| 11. Source Structure, Patterns, Envelopes, and Compression | project_knowledge.md §11; melody_patterns.md source-tree contract and current MUCOM renderer limits |
| 12. Source Loops and Target Loops | project_knowledge.md §12; vgm_loop.md source/target scope; MUCOM timing and loops |
| 13. Source Interpretation vs Musical Restructuring | project_knowledge.md §13 unchanged |
| 14. MUCOM88 MML Is a Musical Target, Not a Segment Dump | project_knowledge.md §14; mucom_musical_projection.md sounding units and structure |
| 15. Target Formats Are Not Canonical Internal Representations | project_knowledge.md §15; mucom_musical_projection.md; output_normalization.md; MGS syntax references in mgs_compatibility.md |
| 16. Validation: Correctness Has Multiple Meanings | project_knowledge.md §16; earlier opening export-statistics distinctions integrated |
| 17. Tests and Fixtures | project_knowledge.md §17; scoped fixture oracle and private-material policy |
| 18. Human-Readable Intermediate Output | project_knowledge.md §18; existing Segment schema and current MUC artifacts |
| 19. Development Workflow | project_knowledge.md §19 unchanged |
| 20. Documentation Placement | project_knowledge.md §20; public principles/specs vs private history |
| 21. Inherited Knowledge and Compatibility Paths | project_knowledge.md §21; verified existing historical notes indexed below rather than public recovery candidates |
| 22. PCM Source Evidence and YM2608 Target Projection | project_knowledge.md §22; pcm_source_and_target.md decoder epochs and current MUCOM bank/profile rules |
| 23. Final Rule | project_knowledge.md §23; evidence, explicit interpretation, late loss and traceability |
| 24. Target Timing, Clock Projection and Playback Compatibility | project_knowledge.md §§7,15; output_normalization.md target timing; MUCOM playback compatibility |
| 25. Whole gates, target controls and native evidence | project_knowledge.md §§4,11,15; MUCOM whole gates/TL/AM restoration; normalization timeline |
| 26. Same-chip projection limits and ordered auxiliary actions | project_knowledge.md §§10,12,15; MUCOM diagnostics/aux actions/indexing; old measurements in existing notes |
| 27. Source structure and target-feasible repeat views | project_knowledge.md §11; melody_patterns.md native vs target-feasible trees; MUCOM composite depth and exact expansion |
| 28. Shared normalization contract and native target allocation | output_normalization.md common contract + intact current MUCOM policy; immutable source/banks, eligibility, atomic fallback, native IDs and catalogs |
| 29. Boundary recovery and native driver initialization | mucom_musical_projection.md boundary recovery/SSG loop proof/native driver initialization; exact arithmetic parity in output_normalization.md; existing boundary note for measurement |
| 30. PCM decoder epochs and live control projection | project_knowledge.md §22; pcm_source_and_target.md decoder epochs and distinct chip runtimes; MUCOM first-note guard/live commands/profile |

## Existing history reused, no duplicate experiments

- 2026-10-07_mdx_musical_output.md: sounding units with held/released trajectories and structural reuse.
- 2026-10-08_psg_opm_musical_connection.md: established musical path rather than tied Segment serialization.
- 2026-10-10_shared_output_normalization.md: source immutability, clock participation and atomic structured fallback.
- 2026-10-12_opna_source_segments.md: authored MUCOMVGM smoke; only missing raw/state/Segment counts and positions from the old public paragraph added there.
- 2026-10-14_native_muc_gate_integration.md: public26 MUC/MUB,20FM and native H/TL ordering probes.
- 2026-10-14_mucom_musical_structure.md:85+1 MUCOM,29MDX, paired auxiliary5;0.1ms native probe versus fixed-clock wider-repeat selection.
- 2026-10-14_mucom_output_normalization.md: two-part8ms policy, public validation and release-control filtering trap.
- 2026-10-14_mucom_boundary_recovery.md: SSG/native driver probes,156checks, synthetic arithmetic speed observation, public native checks.
- 2026-10-15_opna_pcm_controls.md: decoder epochs/live controls, installed compiler/replay profiles and bounded validation.
- 2026-10-15_fm_pan_unknown_diagnostic.md: pending unknown-pan misclassification; no behavior correction in this task.

No new chip support, relaxation or algorithmic improvement is implied by
organizing these documents. output_normalization.md is the only new public
topic file: shared semantics and current MUCOM adapter are explicitly separate.
The inherited MGS normalization doc stays unchanged; copied code does not prove
integration or complete cross-project implementation identity.

## Critical coverage audit

- Observed vs inferred/reset/unwritten facts; raw order, source times and original IDs remain explicit.
- Whole gates vs slices; partial/retrigger/release/zero-time edges remain distinguished.
-8ms floor352, two-part omission+movement; full-mask isolated closed gates only; partial/repeated/unclosed/loop-crossing exclusions.
- Non-key controls inside omitted gates and after earlier retained gates; finite elapsed time after all omissions.
- PCM source eligibility and original bank binding precede omission; inferred natural/EOF closure is not observed reset.
- Exact target domains, protected positive/order intervals, collapsed-control diagnostics, one chosen map and atomic full-baseline fallback.
- Native catalogs vs normalized/timing-feasible views, each projection report, full expanded commands, composite native depth and pre-limit reports.
- Zero accepted-time same-gate initial-pitch donor only; original unknown State and donor provenance; tied code0 spelling guard.
- SSG head+EOF seed proof across all positive intervals, later activation, fixed-envelope observed head restore only; delayed/phase cases remain guarded.
- Native D/E/noise ordering, all-interior interval validation, microbenchmark scope and persisted timeout stage start.
- PCM decoder continuity vs nominal position/natural end; memory/address/data/reset/repeat require separate proof; first-note positive protection and versioned startup profiles.
- Compiler/dialect/runtime/byte/register/waveform/listening and character/binary-size evidence remain distinct; MSX compatibility objective is not certified playback.

Existing internal links to numbered project_knowledge anchors were searched
before pruning24–30 (none found). Local links/anchors and protected-file hashes
are checked separately in the ignored validation report. Historical references
such as old section27 in dated notes resolve via this migration table; they do
not imply that the current core still has that chapter.

Original snapshot SHA256: `51f0582de2ef35aac8b12e5f11bc985ce3471d0c643228c3737c4c129b01bb60`.

## Completed validation

Core1266→673 lines. All280 protected Python/test/README/AGENTS file hashes
match the pre-task snapshot.53 local Markdown references checked:52 public
file/anchor links and1 logical external-field-note reference resolved in the
configured private directory. Task-path git diff --check passes in both repos;
existing intentional Markdown two-space hard breaks remain unchanged.
Architect reviewed old24–30 constraints; clarified two explicit conditions:
EOF same-time zero writes participate in SSG seed, and MUCOM source sample0
maps to target clock0 while runtime startup phase stays separate.
No tests/conversions were needed or run for this documentation-only change.
No stage/commit/push. Unrelated working/staged/private-note changes preserved.
