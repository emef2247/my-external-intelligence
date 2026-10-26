# Editable blueprint from the shared musical plan

## User contract

The user explicitly clarified that timbre and software-envelope definitions
should be removed, but volume changes must remain. Emit an additional editable
MGSDRV MML template under blueprint/, keeping the ordinary products unchanged.
Native PSG/SCC/OPLL retain their lanes; other-source parts use available OPLL,
then PSG. This is not permission to flatten dynamics or discard sounding units.

## Implementation boundary

py/musical_blueprint.py lowers accepted MusicalPlan units and controls without
editing source Segments or parsing the finished ordinary MML. Keep inline v
holds, tied internal pitch/volume continuations, complete-unit finite repeat
planning, then existing lossless macros and allocation/sync annotation. No
SCC wave, OPLL custom-patch/selection, or SW-envelope bank is copied. @0 is an
editable placeholder. SCC needs a user-provided waveform before meaningful
listening. PSG hardware envelopes remain hardware commands.

HuC projection summary now includes native-channel/profile-lane allocation.
The blueprint uses it to route those complete units into free OPLL then PSG,
with affinity preferred. Numeric volume levels survive; new-timbre loudness is
not normalized. JSON/CSV map source, accepted lane and template lane, samples,
steps, per-unit commands, loops, stripping, rhythm source controls and rounding.

Canonical, MGS batch/export and direct VGM export automatically attempt the
additional template. Its publication is independent of ordinary MML compilation.
A failed main backend can still publish a valid template while preserving the
previous listening pair. A failed template must not fail direct VGM. Edited or
unowned templates are preserved and skipped, not allowed to stop main conversion.
Digest ownership survives failed attempts without claiming stale results as new.

## Review corrections and traps

Architect review identified native percussion mid-hold volume loss and edited
blueprints aborting ordinary export. Both were fixed. Rhythm is explicitly an
attack-grid/preset-decay template: native off edges remain inspectable in the
sidecar rather than independent MGSDRV drum gates. A positively held percussion
level change blocks only blueprint; source-ordered same-sample release/setup
transients do not. Never synthesize an extra attack to express a volume update.

Actual MGSC probes exposed a target-duration trap. At tempo225, %1/%2 are invalid;
at75 both are valid;90/120/130 accept1,150/180 require2,225 requires3. This agrees
with the existing normalized renderer bound max(1, tempo//75). A generic note
speller must not emit c192/c96 for one/two steps: those divisors are invalid even
where raw %1/%2 are legal. Also reserve a legal remainder when splitting a long
note into <=255-step pieces. Report a below-bound interval with unit/control IDs,
samples, steps, tempo and minimum. Do not silently add a new60Hz rounding policy
or discard positive volume holds to make the template compile.

## Verification and scope

Evidence is in outputs/diagnostics/blueprint_20261026 (untracked). Final focused
acceptance:95 tests, no failures/errors/skips, including actual MGSC/libkss PSG
volume15->9->3 and full gate duration, long-duration splitting, subframe blocking,
source allocation, repeats, ownership and failure-isolated publication.

The saved60 public accepted plans were used to validate the new backend, not
rerun a broad private regression.58 generate templates;2 conservatively block
held-percussion volume (rhythm_only_test03, sample).50 compile with MGSC and8
hit track-buffer capacity. Inline curves are intentionally not SW envelopes,
so template success is not a guarantee of MGS buffer fit. Fresh representative
public mixed-VGM and OPLL-MGS exports also succeeded. A fresh, previously scoped
short HuC input succeeded with352 sounding units from6 native voices allocated
to6 OPLL lanes. No hardware listening or broad local_only regression performed.

README remained user-owned and unchanged. Public branch codex/musical_HAL keeps
all current HuC/shared-HAL/blueprint changes uncommitted. No staging/commit/push
in either repository; preserve unrelated existing private untracked notes.
