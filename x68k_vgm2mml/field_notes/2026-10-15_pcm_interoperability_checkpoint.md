# PCM interoperability checkpoint (2026-10-15)

User authorized order: shared mucom improvements backport -> DELTA/MDX gaps ->
MSM6258/MUCOM -> SegaPCM/MUCOM. All changes are working-tree only; no stage,
commit, push, private regression or listening. User later added OPM-to-OPNA
consideration; architecture reviewed, no OPM projection implemented.

## Shared and x68k work

Backported pure musical_structure, ordered_timing and opna_adpcm_playback.
MDX wrappers preserve existing domain/clock arithmetic and compatibility API.
Continuous DELTA decoder epochs now preserve source positions across level,
pan and rate changes, including zero-rate pause. Natural exhaustion caps the
waveform and later control changes do not restart decoding. Existing independent
DELTA projection remains. A common pcm_waveform_mdx engine reuses the Sega
packaging/alignment path with separate source labels and diagnostics.

Unsupported repeat/address/memory operations remain blocked. Derived waveform
is a nominal decoder/interpolation model, not hardware DAC/prefetch identity.
Source State/Segments/PCM IR remain intact. No source decoder reset is inferred
from target packaging. Independent target PCM decoder assumptions are losses.

## MUCOM reverse PCM

New pcm_mucom source adapters retain native MSM6258/SegaPCM dumps and create
separate derived ADPCM-B samples, target bindings, K musical units and timing
view. MSM requires known reset, independent playback, regular delivery and
4-bit/10-bit scope; held pan and unresolved source-wide controls are rejected.
Supplied bytes remain distinct from measured consumption (nominal model only).
Sega reuses its existing runtime, with explicit mono/model/loop-freezing losses.

Canonical vgm2mml.py and scripts/export_mucom.py expose --pcm-policy best-effort
and --pcm-only. Mixed OPM extraction is explicit; exclusions are recorded.
No fabricated OPNA source events/registers/decoder epochs are used for target
bank reuse. Native OPNA/ADPCM-B route remains separate and preserves bytes.

MUCOM bank limits:32 slots, four-byte alignment, per-sample length and address
capacity; bounds derive from MUCOM rather than PDX eight-second packaging.
Padding encodes zero-valued target PCM with continuing predictor state, not raw
zero bytes. Valid audio lengths and padding are recorded separately.
Target level249/255 attenuation is explicit. Final MUCOM gate durations do not
stretch encoded samples; target_timing_audit exposes nominal truncation/tails.
No native playback or seamless/phase equivalence is certified.

## Focused evidence (authored/public only)

- Shared allocator12 checks + existing planner equivalence1 passed.
- DELTA waveform4 checks + existing Sega bridge3 passed.
- One authored continuous DELTA canonical conversion produced MML/MDX/PDX;
  inspected one epoch/five control slices, paused position, source IDs and
  final aligned sample count. Initial-pause follow-up used unit check only.
- MUCOM backend5 initial checks + source-wide unsupported-register guard1 passed.
- Existing MUCOM native PCM control1 and musical structure1 passed.
- Canonical public SEGAONE produced MUC/bank with explicit OPM exclusion;
  tiny authored MSMTINY passed batch --muc-only and produced a bound PCM bank.
  Inspected referenced bank address/length, native dump, target IDs and final
  accepted-clock timing audit. These are not MUB compilation/listening claims.
- Final read-only architecture review found no blocking defect; MUCOM timing
  versus encoded duration limitation remains explicit.

Generated public evidence is under x68k outputs/delta_continuous_check and
mucom outputs/pcm_interop_public_check. README links and diff whitespace checked.

## OPM-to-OPNA proposal (not implemented)

Start with at most6 used FM channels and a fixed mapping. Share suitable 4-op
voice/operator/ALG/FB projections, but map operator-bank order separately from
Key-On masks. Derive pitch from OPM KC/KF/source clock into target FNUM/BLOCK.
Keep native OPM Segments and create explicit target FM state; do not fabricate
OPNA source Segments. DT2, noise, effective LFO and other unsupported behavior
must be diagnosed rather than zeroed. Eight-to-six channel allocation and
release tails require separate scope. This is a separate next implementation.

## Next action

User private regression/listening. Default timeout180 unchanged. No resumed VGM
output, compiler capacity optimization or implicit OPM FM exclusion. See both
repos' docs/pcm_interoperability.md and handoffs/current.md for commands/limits.
