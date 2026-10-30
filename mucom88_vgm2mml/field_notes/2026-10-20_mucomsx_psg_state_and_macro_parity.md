# MUCOMSX PSG state spelling and MSX macro portfolio — 2026-10-20

## Scope and prior decision

User explicitly requested implementation/reconversion after the MSX docs and
private-note audit. Public fixtures only; no local_only conversions/listening.
No stage/commit/push. Source Segments and musical-note boundaries stay intact.
Read previous policy audit before this implementation; do not reinterpret this
as permission to flatten source notes or infer envelope tails.

## Saved implementation

PSG musical units still come from MSX extract_notes and prepare on source-sample
views. Keep complete note/rest grouping and observed run lengths; target
normalization is separate. Sequential state spelling now precedes exact
source-plan projection, as in MSX. P/D/o/software-v duplicates are omitted,
relative octave and exact C128 duration aliases shorten text. Preserve all
E/w/m/S and ties. S invalidates remembered software-v. L forgets all known
state even for a carried gate. This lets first-pass initialization differ
without changing the exact finite-loop expansion contract.

Before/after commands, removed setters and generated-subset effective-state /
ordered-action proof are dumped per unit. Native Segment/note dumps retained.
Architect review found no concrete blocker. Local mucomDotNET music2.cs
corroborates base-volume retention through ENVPST/rests and per-channel mixer
masking, but is not a proof that every MUCOMSX runtime detail is identical.

Macro search now matches adopted MSX allocation settings: atomic baseline plus
(24,0),(64,0),(64,1),(64,2); max 32 definitions, max 512 searched body characters.
Merged-track sibling/loop-child AST candidates preserve tie/L barriers and
ancestor nonoverlap; definitions contain no calls. Every alternative proves
exact expanded tokens/loop tree; baseline wins ties. Search measures combined
logical definitions/tracks before physical wrapping;1023-byte line limit is
separate. Text compression does not imply compiled DATA compression.

## Software-envelope support boundary

Pinned vendor MucomDotNetExtension/MUCOMDOTNET_EXTENSIONS.en.md70–76 documents
@n=al,ar,dr,sl,sr,rr presets (0..15), hardware S/m, software volume LFO and
v restoration of ordinary volume mode. No arbitrary tick-table playback or
parameterized note macro was established. Thus curves are still tied v controls;
no ADSR fitting/unobserved tails or native arbitrary-envelope parity claim.
Shared literal command sequences can become macros. All E attacks remain.

## Public evidence (final totals to be appended)

97 distinct focused public/synthetic checks passed: 95-test MUCOMSX suite and
2 added note-rest-retrigger/relative-octave repeat cases. All51 requested public
inputs are being reconverted in a fresh directory. Actual MUB compile-only jobs
use isolated copies of the authorized DOS image; no source image modification.

Root: outputs/mucomsx_psg_compaction_20261020/
The before_state baseline already uses the new macro portfolio. State spelling
comparison: psg_scc_001 26617 -> 22281 bytes, psg_scc_002 8701 -> 7972 bytes. Do not
attribute those numbers to the portfolio change, or treat them as private-song
regression. comparison.json independently hashes source Segment/note dumps.

## Native VGM probe limitation

An authored pure-PSG explicit-E score and an inline @0=.../@0 experiment both
compiled CHECK.MUB/SESSION.MUB recognized msPb0100 (2412/2414 bytes), DONE.OK.
Full runtime export left CHECK.VGM 160 all-zero bytes and failed final-header
validation after 180s. The original-E control also failed, so this does not
establish a regression caused by state spelling or a preset-specific failure.
Vendor MSX_TARGETS says AY 0xA0 / OPLL 0x51 export is supported; root cause not yet
established. Do not describe native export as unsupported or playback matched.
The experimental preset representation was not adopted. No runtime guard was
relaxed. Full artifacts in preset_probe/ remain inspectable.

## Remaining boundaries

No timing change to force exact source-sample repeats into MGS-style60Hz loops.
No capacity bypass. Long mixed/public or private songs may still exceed MUC /
compiled-track limits. User owns private regression. Suspended enhancer and
OPLL custom/preset identity work remain outside this task.

## Native OPNA SSG corroboration

A separate authored OPNA SSG D-part score exercises note/rest/retrigger,
relative octave, same base volume and held decay under the same state-spelling
pass. Original and optimized scores both compiled/exported successfully with
MUCOMSX. Exported VGM before/after:1367/1358 bytes, OPNA-port0 writes219/216,
same168 waits and27552 total samples. Deduplicated effective SSG register-state
trajectories are identical (7 state changes). This supports the generated
state proof for the SSG compiler route; it does not resolve the internal-PSG
native-profile export failure or certify all possible driver behavior.
Artifacts: ssg_state_probe/comparison.json and both validation.json/VGM files.

## Saved low-allowance checkpoint and exact search speed

User requested code saved at10% remaining. Saved code/docs/handoff already
notified. Full public run completed38/51, all38 successful MUCs; native
compile-only35/35 passed. Large OPLL-heavy public tracks spent minutes in
exhaustive macro allocation. That old process was stopped explicitly, leaving
its source/structure/results intact. No outputs were removed.

A lossless repeated-prefix enumeration is now saved: across every sibling AST
location, singleton prefixes cannot have repeated extensions. Only repeated
prefixes are grown. Ineligible tie endpoints are retained during extension so
legal longer candidates are not lost. Bounds, candidate set, ranking and four
allocations remain unchanged.16 focused tests passed;24 random nested-tree
cases times3 initial ranks matched original exhaustive candidate sets and
complete output/reports. A synthetic4000-token benchmark improved1.6668s to
0.0172s, with identical7916-byte selection. This is a synthetic timing, not
a promised speedup for every song.

Remaining13 public fixtures restarted in remaining/; first38 results remain
in after/. Current distinct focused test count98 (prior95 plus2 state boundary
cases and1 search-equivalence case). SSG native MUB376->352, VGM1367->1358
bytes, identical effective states and27552 total samples.
