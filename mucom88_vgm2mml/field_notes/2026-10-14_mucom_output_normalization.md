# Native MUCOM output normalization and shared architecture

The user requires --normalization-ms, default8ms, to implement the inherited
TWO-part policy: original-time complete gate omission and bounded target-time
movement. A boundary-only Timer-B limit was not that integration. A clean copy
alone cannot ensure the new MUC entry route executes shared musical stages.
Native reader compatibility or compiler success is not an integration proof.

Common pieces now reused: normalization_samples, source attack infer_timing,
ordered integer-domain allocator (also used by the retained MDX wrapper), and
musical_structure.source_plan for native, timing-feasible and normalized views.
MUCOM supplies Timer-B integer domains, C128 candidate adaptation, MUC syntax,
source-chip event adapters, banks and native compiler limits. MDX/MGS clock or
syntax facts are never imported as MUCOM rules.

Only complete canonical full-mask observed FM on/off pairs <=floor(44.1*ms)
qualify; 352 qualifies at8ms,353 does not. Partial/repeated/unclosed/loop-crossing
operations and internal slices of held gates are retained. Original analysis
and PCM IR stay immutable; output-only replay preserves native IDs/intervals
and non-key writes. PCM eligibility and bank binding precede omission. Only
independent single observed-reset PCM epochs qualify; inferred natural end and
EOF do not. Original bank bytes and bindings remain even after target omission.

Exact rational native periods define integer movement domains. Ordered DAG
allocation retains positive gates, partial operations and protected rests/
trigger/loop spans. Candidate inference confidence is not output tolerance.
Adopt omission+timing atomically or restore baseline. All-short output retains
finite song duration. Rendering/validation consume recorded clocks and explicit
omission IDs, including original IDs in repeated source loops. Missing timing
samples cannot silently fall back to nearest rounding.

Native structural catalogs remain intact. A separate accepted-clock semantic
FM/rest view removes original jitter durations while preserving attack/held
identity, full parameter sequence and membership. Auxiliary internal timing
keys remain conservative pending domain-specific adapters. Every emitted repeat
expands to the same projected command sequence.

Review trap: using first_omitted_id as the sole release filter drops edits after
an EARLIER retained gate. Preserve OR of baseline and omitted-gate eligibility.
FM native verifier excludes released envelopes, so inspect source-linked output
control rows independently; added long note/off/TL39/later-short-note test.

Validation:138 focused tests pass; public42 MUC/MUB success and36 native FM pass
include16 duplicate fixture copies. Added real native silent/mixed FM checks,
352/353, partial/retrigger/EOF/loop protections, impossible candidate fallback,
source-end mismatch, zero pulse, short-reset PCM IR/full-bank-byte retention.
WSL mixed_intro finite export works with8ms, --no-song-loop, --timeout600.
Artifacts: mucom88_vgm2mml/outputs/diagnostics/mucom_normalization_revision/.
No private-song conversion/replay/listening. No64KB-resolution claim.
No stage/commit/push. User performs private regression next.
