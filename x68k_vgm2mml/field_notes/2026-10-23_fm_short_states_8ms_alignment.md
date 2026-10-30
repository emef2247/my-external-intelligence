# Shared FM short-state selection: 8 ms default

## User decision and prior evidence

The user reran KNIGHT with best-effort PCM: 13/18 success; 03, 10, 11, 13,
16 still compilation_failed. 14 now succeeds. They requested widening the
intra-note approximately 1 ms selection to 8 ms, retaining room for 12 ms.
No new local_only conversion or listening was performed by the agent.
Prior failures used MDX track offsets beyond 0xfffe; no claim of a new
private result or guaranteed size fix is made.

Read inherited MSX notes 2026-10-01_sample_note_length_experiment.md and
2026-10-02_sample_vgmticks.md. Their 44-sample diagnostic tolerance was
experimental and did not redefine source evidence. The subsequent production
selector used 44 samples. This new user-authorized output policy raises that
selector's default; it does not retroactively alter those experiments or the
independent reference-comparison filter. A 60 Hz design preference is not a
universal hardware interrupt minimum or proof that every short control is
inaudible.

## Saved implementation

Identical py/fm_state_runs.py in msx, x68k and mucom repos. Default threshold
352 original 44.1 kHz samples; configured 12 ms gives 529. Never discard all
states of an attack. MSX existing opt-in normalization uses the common default;
x68k and MUCOM use their existing --normalization-ms setting. Normalization OFF
bypasses target short-state selection. Whole-gate omission, boundary movement
and intra-note state selection remain distinct policies using this setting.

x68k structured ordinary complete gates: group full states by original sample
timestamps before target tick grouping; eligible pitch and uniform carrier TL
runs may be dropped. Retained first state backfills a removed prefix; preceding
state fills a removed middle. Protect tone/pan/modulation, nonuniform operator
levels, partial/repeated key operations, raw/global ordered effects and zero-time
terminal operations. Preserve complete key endpoints. Last positive state is
protected for release; if it maps to zero target duration, reject that gate's
short-state plan instead of losing its release registers. Rescue of same-target-
tick setup rechecks pitch/noise/voice capacity. Rejected omissions are marked
not_applied. Source State/Segments/PCM IR are unchanged. Existing exact performed
run grouping and loop planner consume the selected target view.

Native OPM and OPNA-to-OPM share convert_opm; explicit authored OPNA fixture
verified 400-sample pitch transient retained at 8 ms and omitted at 12 ms, with
separate JSON/CSV and native evidence. MUCOM native OPNA and mapped OPM-to-OPNA
share its FM gate adapter; its uniform TL eligibility covers all four operators.
Release-tail protection exists there as well. This is common selection with
chip-specific conservative guards, not identical source FM representations.

Always-saved x68k .fm.short_states.csv/.json report threshold, protected short
states, applied omissions and rejected plans; pass dumps additionally retain
.mdx.structure.short_states.csv and the original trajectory/performed runs.
MUCOM retains .mucom.fm_states.input/output/decisions.csv.

## Verification

x68k focused tests: 7 short-state tests (8/12 ms endpoints, disabled case,
source preservation, controls, prefix/release, zero target release fallback,
and compiled full-state replay against an explicit retained-state expectation),
12 performed-run, 12 renderer, 7 note-normalization, 4 OPNA projection checks.
MSX note normalization: 13 checks. MUCOM: 37 checks (8 FM-state, 16 normalization,
7 FM edge, 6 OPM-to-OPNA). Private fixtures excluded.

Public OPM verifier: initial run 43 inputs, 42 compile/replay successes and one
existing active OPM+SCC preflight block (from_psg/block_boundary). Final rerun
after the release-tail safeguard also has42success/1existingpreflightblock;
all42supported comparisons passed. Supported
public examples exercised short-state omissions; native acoustic identity is
not claimed. Artifacts: outputs/diagnostics/fm_short_states_8ms_20261023/.

Architect found the zero-target-duration release-tail bug; fixed with explicit
fallback and a regression case. Final review found no additional material issue
within the limited pitch/uniform-level scope. Code/docs saved, not staged,
committed or pushed. Public/source repositories and this external note repo
remain separate; unrelated private notes were preserved.
