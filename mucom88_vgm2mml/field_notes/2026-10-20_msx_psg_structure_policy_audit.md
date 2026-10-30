# MSX PSG structure/envelope/macro policy audit — 2026-10-20

Scope: user requested close reading of established MSX docs/field notes and
checking existing results. Read-only source/artifact analysis. No production
code edits, conversion, compiler, playback or test execution in this audit.
A previous approval-review usage failure prevented the first read attempt;
this subsequent approved read succeeded. Do not treat the earlier provisional
explanation as a completed audit.

## Authoritative policies read

MSX docs/project_knowledge.md, software_envelopes.md, melody_patterns.md,
structured_macros.md, mgs_melodic_projection.md; related PSG/SCC Segment docs.
External MSX notes: 2026-10-03_psg_scc_pre_envelope_structure,
2026-10-03_structured_macro_experiment, structured_macro_followup,
default_compression_opll_mode_titles, 2026-10-05_project_knowledge_migration,
2026-09-29_macro_compression, 2026-09-30_nemesis_macro_capacity.

The adopted order is complete interpreted notes/rests -> reversible source
structure using pitch/settings/volume trajectories -> shared software-envelope
bank selected on stored representatives -> stateful rendering -> exact loop
projection -> structured macro allocation. Do not make one note per Segment,
invent attacks/notes to enable compression, use reference score boundaries, or
discard source evidence. Pitch writes, rises, relevant hardware-envelope resets
and meaningful state changes retain boundaries under existing interpretation.

Envelope selection reserves ID0, at most32 definitions, varying long observed
curves first, exact observed-prefix reuse, explicit tied volume fallback if no
representable/selected definition exists. Those MGS envelope/hold syntax and
60Hz semantics must not be pasted unmodified into MUCOMSX. Reuse musical
interpretation/curve selection, adapt target representation with proof.

MSX structured macros compare baseline plus width24-first0, width64-first0,
width64-first1, width64-first2. Each candidate body <=512 characters, up to32
definitions, balanced tree traversal and complete-output character comparison;
expanded commands must match. Source vs compiled capacity are separate.

## Confirmed current implementation differences

mucomsx_source.py imports extract_notes and prepare, constructs sample-clock
views retaining immutable native Segments, and performs note grouping before
rendering. It is not literally one MML fragment per native Segment.
mucomsx_conversion.py only dumps candidate_curves and structural occurrence
counts; it does not call the shared envelope bank to select/output reusable
software-envelope definitions. Rendering uses a flat E and inline tied v
updates. Thus source volume trajectories are present, but shared-envelope
compression has not been carried across. Earlier claims of full common
interpretation/structure support were too broad.

The MUC PSG renderer emits a flat E/mode/pitch prefix at each noncontinuing
unit and absolute volume/octave/detune controls, unlike the MSX emitted-state
tracking that prunes equal setters and uses short relative changes. Its macros
spend slots on repeated setup fragments as well as music.

MUCOMSX macros use the exact candidate helper but compare atomic and token-tree
greedy searches at width64; they do not use the MSX four-allocation portfolio.
The 1023-byte MUC physical-line bound is used for candidate body capacity rather
than the MSX 512-character search policy. Token-tree search starts in each
rendered unit separately, whereas MSX traverses complete synchronization blocks.
These differences are confirmed; their individual byte contributions are not
isolated measurements. Match the agreed selection policy with target grammar
and physical limits kept distinct.

## Existing FFMSX07 evidence (no rerun)

MUC 44406 bytes,32 macros,no shared @e-style curve definitions. MSX MML6413
bytes,32 macros,9 envelope definitions (including constant ID0). MUC report
macro measure: 91143 ->43057 bytes (tracks plus macro definitions), saving48086;
full MUC includes headers/wrapping. Macro substitution is active and materially
reduces text; lack of macros alone is not the cause of final expansion.

PSG native Segment rows3798; interpreted notes/rests3020, sounding2727;
338 sounding units aggregate multiple Segments, all338 with varying volume
runs. Envelope candidates149. Target PSG units3025, sounding2040 after output
normalization/splitting. These are distinct stages/counts, not equivalent note
or KeyOn counts. All three PSG tracks report zero emitted finite repeats in
this saved run; source planning exists but no physical loops survive there.
Do not infer a specific timing/loop-selection cause without inspecting source
candidate and target projection details. Current target time base and replay
extent also need matched comparison against MSX before attributing all growth
to one stage.

## Required direction

Honor the user's request to follow MSX musical-unit, shared-volume, structure
and macro selection policy. Fix missing representation stages before treating
a larger macro budget or source capacity bypass as a solution. Keep all source
membership/trajectory dumps and expanded-semantics verification. Target support
for exact shared software curves must be established rather than assumed.
No new heuristic or code change was made in this read-only audit. No Git
stage/commit/push. User owns private conversion/listening regression.
