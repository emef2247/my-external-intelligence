# Playback-budget proposal after the PDX census (2026-10-27)

Investigation only, requested while user cannot yet audition the MSX HuC work.
Reused pdx_library_20261027 census and current x68k target code; no conversion,
library rescan, playback, driver changes or production/default edits. Preserve
all pre-existing uncommitted MSM6295 work. Ignored Japanese proposal:
outputs/diagnostics/pcm_capacity_plan_20261027/analysis_ja.md.

Census844 validated PDX, median49750B/p95319423B,817 single populated banks and
slot median3784B support prioritizing reusable assets, not a universal format
limit. Identified MXDRV Rel.X5-S initial319488B minus MXCTRL270B yields319218B
conditional file allowance. Actual post-replacement driver/loader/allocation
remains unknown; latest observed screenshot was MADRV. Never infer its budget
from the MXDRV reference. Current1200000B NanoDrive8 policy stays independent.
Web recheck of MXCTRL LOAD_PDX/TRANS_PDX confirms hedlen270 and full len+hedlen
transfer; cache truncates MXDRV source before its Start, so312KiB rests on prior
saved primary-code investigation, not newly recovered web lines.

Current pcm_capacity and consumers import the fixed1.2M separately. budget96
already enforces slots AND bytes via selection of existing assets; it does not
pack all candidates then discard a completed file. pcm_bank_budget _select is
usage-weighted facility coverage, feasible by encoded cost but not marginal
benefit per byte; onset-audible repair does not guarantee long-tail coverage.
Common-bank branch in opm_conversion includes SegaPCM,C140,MSM6295,nativeMSM,
projected OPNA. PSG continuous-mix override is excluded from approximate sharing,
though final PDX cap/profile conversion apply. Current psg_pcm_mdx has a63tap
Hamming-sinc filter; do not repeat old blanket claim that all routes lack an
anti-aliasing filter. low/ultra source profiles elsewhere have separate limits.

Propose one immutable resolved target capacity policy, threaded from canonical
CLI/batch through source-validation target view, representative selection and
repair, force, bindings, packing and stats. Effective file budget=min(1200000,
resident-wrapper-configuredmargin). Reject impossible table-sized budgets; final
whole file check includes table/padding/tail. A provisional300000B PDX target
could be offered ONLY for explicit MXDRV312+MXCTRL assumptions, not a universal
new default nor a runtime guarantee. PDX capacity check, estimated transfer fit
and independently tested playback remain separate. Temporary file buffers,
driver workspace and other residents affect peakRAM separately. Raising resident
allocation can preserve fidelity if actual available RAM permits.

Order for later optional best-effort auto-fit: exact byte reuse/unused removal;
bounded approximate sharing; evaluate original-derived standard/low/ultra
candidates; capacity-forced substitution if still required, with affected time,
quality, material classes and tail coverage visible. Not cumulative transcoding
and not a score-time/gate change. Strict cannot silently become lossy. Counts
alone favor tiny misleading representatives; protect long phrases from the
short-drum heuristic. Conversion-first preference persists, but inability to
make a valid fitting asset must not be reported as verified playback success.

User-reported SALAMD01 payload693570+table768=694338B gives purely arithmetic
~348KB at half rate/~174KB at quarter rate: half still beyond conditional312KiB,
quarter potentially fits. Not a new conversion result. Adaptive rate is useful
for continuous mixes where repeated MML notes do not reduce baked audio bytes.
PSG source-aware bank extraction is a later separate scope because shared noise
and autonomous HW-envelope continuity must survive note boundaries.

EX-PDX, more banks, LZX or a different packer do not by themselves reduce a fully
loaded expanded PDX. mkpdx68 remains optional packaging, not memory-fit logic.
Do not mechanically split ADPCM into64KB chunks as a capacity remedy.

Focused architect review agrees; cautions recorded: actual environment first,
peakRAM distinct, propagate SAME budget everywhere including representative
repair/force, approximate gain sharing isn't lossless, retries from original
IR, and onset preservation isn't complete-gate preservation. No implementation
started or committed. Next: user decision on the proposed capacity stage or new
actual-driver feedback; preserve existing relaxed1.2M policy until authorized.
