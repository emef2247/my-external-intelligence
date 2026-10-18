# Explicit partial OPNA/PCM conversion (2026-10-16)

## Latest user scope

--force aims to finish processing and produce audition candidates despite
unfinished target conversion. Do not restrict force to SSG or only known register
holes. Clearly list skipped register effects after conversion. User additionally
requested PCM capacity omission: keep what fits and skip later samples/notes.
User explicitly allowed compiler fallback to be deferred; this checkpoint
defers it. Private regression/listening remain the user's responsibility.

## Hardware policy

Normal/default failures remain. Explicit force skips native unmodeled OPNA
register effects (including, but not limited to, unallocated operator-address
slots). Native write classification/evidence is unchanged and saved independently.

CH3 special/CSM gates only CH3 output for unsupported intervals. FM SSG-EG gates
only affected FM channels. Ordinary explicit keys and supported controls remain
underneath the gate; restoring pan does not synthesize an extra Key-On.
Repeated pan writes cannot reopen an unsupported channel. CSM's native generated
keys/envelope phase remain unmodeled. Record modeled-but-unprojected timers,
special frequencies, SSG-EG, rhythm/IRQ and LFO approximation distinctly.

SSG unsupported intervals preserve original Segments and elapsed time. Diagnostic
feature register may have been configured before the boundary event;
trigger_register and target-feature register are separate.

Known unsupported DELTA hardware/control cases omit the whole PCM part using
an explicit projection-only view, never overwrite original native IR.
Missing evidence, invalid clocks, unknown source diagnostics and malformed source
stay blocking. Ordinary held-level/pan/rate/zero-DeltaN remains supported.

## Capacity policy (X68000 PDX)

First eligible playback-use order binds whole source sample IDs. Keep up to96
distinct samples within the currently verified65535 encoded-byte sample scope.
This is not a PDX format byte ceiling, and no MUCOM256KiB cap is imported.
Oversized samples consume no slots. Once slots fill, new samples are omitted;
later references to already-bound samples survive. Rejected notes become timed
rests and retain final end/L, including loop-crossing and trailing omissions.

Normalization-only short-note omissions do not occupy slots; their original
evidence is still validated and reporting distinguishes them from capacity loss.
Force authorizes capacity loss only, not all unrelated strict-policy losses.
All-PCM-omitted paths avoid empty PDX and preserve unowned binaries; CLI never
prints stale retained files as current generated output.

## Diagnostics

Canonical CLI, batch export after each input status, and TXT list port/register, affected bits, action, count kind and reason.
Full .opna_force.skipped_registers.csv/json preserves source event/provenance,
source intervals and loop occurrences. .pcm.force_capacity.csv/json records
retained/omitted sample/playback IDs, exact bounds, actual/limit and scope.
OPNA PDX sample count is updated from final capacity-selected timing report;
pre-capacity candidate_pdx_samples remains separately named.

## Verification and review

No private conversion/regression/listening or compiler execution.
Root's six authored force checks passed, covering unknown-register opt-in,
CH3/CSM/FM SSG-EG gate/recovery without new keys, source evidence, CLI listing,
rejected-retry cleanup and null-PDX artifact reporting.
Worker's six PCM-HW omission checks passed.
Worker's seven initial capacity checks plus one new normalization-slot check
passed; only these focused authored checks were used.
One persistent authored canonical CLI produced MML/native evidence/decision CSV/
JSON/TXT under outputs/opna_force_check. Inspected registry including both
unknown-register effects and SSG interval feature decisions.

Focused architect review found and corrected: null pcm_pdx_file CLI exception,
pre-capacity count reported as actual PDX count, and normalization-omitted samples
consuming bank slots. Gate/provenance validation had no other reported blockers.

Working-tree changes only. No stage/commit/push/branch operation.
Next: user retries SNATCH07 with existing --force command, then owns audition
and regression. Compiler/track/loop-format limits can still fail; do not silently
declare them successful or resume compiler optimization.
