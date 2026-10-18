# PSG replay regression: compiler capacity and next roadmap (2026-10-16)

User reports latest DSLAYER4 export after carried-state replay:17 success,
3compilation_failed. Their introductory prose says11/14/15, but actual pasted
logs and saved reports identify11/14/17;15 succeeded.05 now succeeds;14 no longer
fails conversion at a held loop boundary. User treats compiler capacity as
lower severity and requests confirmation/recording, not optimization now.

## Read-only confirmation

Existing diagnostics inspected, no source reconversion/test/compiler/listening:
- DSLY4_11/run_96x__vex: compiler line1495 column48.
- DSLY4_14/run_4a4zg7yq: compiler line1497 column56.
- DSLY4_17/run_3h6zatp0: compiler line1491 column62.
Paths are under outputs/psg/vgmrips.net/DSLAYER4/_diagnostics/.
All say generated performance data exceeds maximum64k. All manifests status
converted, stage musical_structure_and_rendering,2source traversals,0force
omissions. MUC exists; no MUB is published for failed compiles. This is not PCM
bank capacity, a conversion exception, timeout or another missing-clock error.
MUC text byte sizes are640138/677732/415678 respectively; these text sizes are
NOT measured compiled performance sizes or excess-byte counts.

Emitted finite repeat commands by D/E/F/G:11=26/27/28/2,14=2/4/0/4,
17=0/0/0/2. Recorded14 E/F seams stable_carry;17 D stable_carry. Compression
strength differs, but these counts alone cannot establish musical/planner
superiority or prove an optimal size. Two traversals increase represented
material; no controlled old/new compiled-size comparison was performed.
Do not silently remove initial traversal, disable looping or loosen fidelity.
Compiler choice/format and semantics-preserving compaction are future options;
no claim made that another compiler necessarily removes the64k constraint.

## User blueprint and checked structure boundary

User wants msx/x68k/mucom to converge on consistent source-derived musical
interpretation/structure, then eventually backport proven shared improvements
into MSX. Wants x68k PSG/OPNA SSG->OPM to follow note/run interpretation, revisit
OPM noise, compare FM structure before any rewrite, and begin independent
OPM->OPNA FM conversion without vgm-conv dependency.

Read-only code comparison: MSX PSG/SCC extract_notes/settings/volume_runs feeds
LoopFirstEnvelopeBank/SourceLoopPlan before MGSDRV envelopes. OPLL instead uses
opll_note_units.group_notes with Key gates/patch/sustain trajectories and
OpllLoopPlan outer/inner validation. They share the musical-unit-first approach
and lower-level structural infrastructure, not identical unitization.
X68k OPM has native key/register trajectory units, inner/outer planning and
musical safe-span rendering; it is not established as globally better or worse.
Mucom reuses MSX extract_notes, but keeps raw control/context structural keys
and MUC-specific renderer/seam handling. Do not claim full identical MGSDRV
compression or use these compiler failures as a controlled comparison.

## FITOM reference review (read only, no code copied/executed)

https://github.com/madscient/FITOM_staging/tree/main/tools/voice_convert
README.md, HANDOFF.md, fb01_convert.py, necopn_convert.py and
https://github.com/madscient/FITOM_staging/blob/main/docs/voice-parameter-reference.md
were fetched. Latest tools/voice_convert commit observed via GitHub API:
0153cf17efda3c4904ebd28f83639629dde13202.
These convert static synthesizer voice formats to FITOM common hwbank data;
they do not implement OPM VGM event-stream -> OPNA conversion. Useful references
are operator order, raw field meanings/bit widths and OPN/OPM differences.
FITOM-specific carrier/velocity normalization and HW-LFO omission are not
policies to copy into a source-faithful VGM converter. Preserve State/Segments;
map target voice/operator/control events and pitch separately. Held edits and
partial Key operations need explicit projected trajectories, not just onset
voice mapping. Nonzero DT2, LFO/noise differences need diagnosed policies;
OPM8->OPNA6 shortage retains user's agreed explicit omission option.

## Noise scope and recommended sequence

YM2151 noise uses channel8/operator4 (0-based ch7/C2), as documented by Yamaha
YM2151 synthesis manual and Furnace's own YM2151 manual:
https://map.grauw.nl/resources/sound/yamaha_ym2151_synthesis.pdf
https://raw.githubusercontent.com/tildearrow/furnace/master/doc/7-systems/ym2151.md
AY noise is shared across its channels, but channel tone/noise gating/volume
makes arbitrary mixtures more complex than one noise voice. A possible first
scope is one active noise-only source interval -> reserved OPM noise carrier,
with period approximation, allocation conflicts and loss reporting. Not yet
implemented or acoustically validated.

Recommended incremental order after current user commit: small source-unit/key
contract comparison (read-only findings now available), OPM->OPNA minimal6ch
basic4op/pitch/Key/held-control implementation, x68k PSG/SSG musical-unit reuse,
scoped noise support, then measured FM structural improvements and MSX backport.
Do not make a large common-base rewrite a prerequisite for useful OPM output.
Architect review supports separating static voice mapping, event projection and
shared structure; no wholesale FM rewrite justified by current evidence.

Public source/docs/tests and Git index were not modified in this investigation.
This external note is separate from the user's public commit/PR.
