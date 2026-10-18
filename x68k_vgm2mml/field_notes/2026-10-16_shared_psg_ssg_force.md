# Shared PSG/SSG force omission (2026-10-16)

## User decisions

User asked to confirm SSG/PSG common conversion, explicitly declined new noise
support in that exchange, then requested unsupported HW envelopes/noise/channel
shortage also be --force omissions with skipped diagnostics.

Inspection: OPNA SSG already shared PSG FM tone model. OPNA force already muted
noise/EG and registered capacity-dropped parts. Standalone PSG/SCC MDX rejected
--force at frontend and had no common projection force path.

## Changes

Common psg_scc_opm.project force-only suppression now marks unsupported PSG
noise/EG intervals inactive without rewriting their source fields or times.
Both FM and additive PSG models use the same omission. Ordinary force-disabled
rejection is retained. SSG delegates to the same common decision and enriches
its omission provenance with native event and loop IDs.

Standalone PSG source conversion forwards force, retains all original source
pass dumps when force, and writes .psg_force.skipped_registers.csv/json. Source
PSG Segments do not expose original event IDs, so those fields remain unknown;
source row and native boundaries identify each omission. Register-state
references are marked segment_state, not fabricated observed writes.

Shared register summary now includes source_part, and displays PSG1/SSG3/FM3
rather than requiring users infer the affected part from register addresses.
The OPNA FM-priority whole-song allocation is unchanged, including its existing
explicit capacity exclusion; force shows those parts in its aggregate summary.
PSG and SSG source count constraints are not redefined as target capacity.

## Verification

Changes saved before checks. Only four authored checks ran:
new shared PSG model suppression (FM/additive, original source preserved,
proper inferred release/restart), authored canonical PSG CLI force success and
strict retry clears diagnostic artifacts, omitted SSG3 capacity part label,
and one existing authored SSG source/gate preservation case. All passed in2.697s.
No private conversions, broad regression, listening or MDX compilation.
No stage/commit/push/branch changes. User owns regression.


Persistent authored evidence: outputs/psg_force_check contains canonical force
MML, native Segment CSV retaining noise/EG flags, omission CSV/JSON and TXT.
Inspected original fields against target omission records. No private data or
external compiler was used. Windows git diff --check passed.
