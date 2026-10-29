# 2026-10-29: blueprint indentation and self-written MIDI

## Scope and loop-setting clarification

User is comparing SALAMD default/legacy-loops/score-guided output and v1.1.0,
and will supply results. There are currently three behaviors: default
LoopFirstEnvelopeBank; legacy EnvelopeBank with older OPLL loop lowering; and
score-guided native-score proposals plus exact-command fallback. Guidance keeps
attack versus continuation and does not indiscriminately erase articulation.
The earlier promised default-guidance change was not applied after an approval
service quota error. Preserve current defaults pending the user's comparisons;
do not mislabel this blueprint work as a loop/macro quality improvement.

## Presentation defect

native_blueprint.finish applied mml_notation.compact after format_mml_loops.
The compactor rebuilt every physical track line with one space and strip(),
destroying nesting indentation. Preserve original post-designator whitespace
without moving boundaries (caret length additions depend on physical lines).
Authored nested-loop test forces actual shortening so unchanged-text fallback
cannot conceal the bug. Saved SALAMD01 blueprint: 0->95 indented lines; decoded
score unchanged; both compile to identical MGS bytes; original hash unchanged.
No private source VGM reconversion or corpus sweep was performed.

## MIDI projection and contract

New blueprint_midi.py uses only struct/Fraction/stdlib to encode SMF type1,
480 PPQN, normally10 ticks per MGSDRV score step. Existing handwritten MIDI
utilities in opll_vgm2midi/py/midi_utils.py and vgm2tx802 were inspected as local
references; no runtime dependency or synthesis/timbre mapping was imported.

native_blueprint records score_notes/end_step from the same post-clock,
post-pitch-fold spans as MML. Native source stays intact. Equal pitch within
one continued unit remains held; distinct units retrigger, with MIDI note-off
before note-on at equal tick. Pitch-changing continuations are separately
identified in the sidecar; neutral MIDI is not a chip-legato emulation.
Finite repeats are already expanded in these spans. Every track/conductor ends
at the score page endpoint, retaining trailing rest. All melodic parts use
fixed velocity96 and program0; no source volume/control curves are restored.
MML remains authoritative for documentary native timbres/waveform parameters.

One .midi per .mml page, including .partNN siblings. PCM remains excluded;
non-OPLL percussion retains the existing explicit blueprint limitation. OPLL
rhythm uses GM35/38/41/49/42 on channel10 with a short fixed audition gate,
not independent native stop/decay. More than15 melodic parts use MIDI Port
metadata before channel reuse, preserving equal-pitch independence. Record all
source-part/track/port/channel mappings. Score tempos1..3 require explicit
metric scaling in tempo and ticks to fit SMF's 24-bit tempo range; physical
time is retained and the scale is reported. Track text is documentary UTF-8.

## Publication

Blueprint MML/MIDI form one optional owned bundle; edited MIDI protects it as
edited MML does, without blocking ordinary/reference products. Older ownership
records without MIDI migrate when no unowned sibling exists. Keep report.files
MML-only; add midi_files with SHA/metadata. Batch validates paired counts,
paths and checksums, publishes both, removes owned stale split siblings and
records blueprint_midi_files separately. JSON score_notes and notes.csv expose
writer inputs. Direct rollback now preserves disk backup/recovery metadata if
restoration also fails. Never report retained old products as newly generated.

## Validation and limits

136 related authored/integration tests passed: SMF VLQ/data ranges, tempo,
endpoints, retrigger order, continuations, multiport voices, drums, owned edits,
split shrink, malformed/missing optional pages, rollback, actual MGSC replay,
public exports and independent production/blueprint clocks. Public PSG scale
export succeeds with MML/MGS/nativeVGM/blueprintMML/MIDI. An independent existing
vgm2tx802 SMF decoder reads type1,2tracks,480PPQN,15 note onsets/pitches exactly
matching the score and both EOTs at11550 ticks. No MIDI player listening claim.
Ignored evidence: outputs/diagnostics/blueprint_midi_20261029 (layout summaries,
unchanged source hash, compiled MGS comparisons, actual public products).

README unchanged. No commit or push requested. Wait for user SALAMD regression
before changing macro/loop defaults or claiming improvement over v1.1.0.
