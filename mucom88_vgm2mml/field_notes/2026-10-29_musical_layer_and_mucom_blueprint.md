# MUCOM musical-layer and blueprint port — 2026-10-29

User requested preserving the existing MUCOM repository while importing current
MSX native musical-layer sources, reconstructed native VGM, blueprint and option
forwarding. User explicitly confirmed blueprint should be MUCOM88-format MUC,
including when the production target is MUCOMSX. MSX changes were committed as
b78d675, with separate private checkpoint 2f43827. MUCOM edits remain uncommitted;
preserve earlier MDX removal/curve backport and unrelated -w.mub.

Namespaced HAL source provenance now points to MSX b78d675. Native controls use
integer44100Hz VGM samples. Imports include PSG/SCC/OPLL/HuC6280/OPM/OPNA/DCSG/
MSM6258/MSM6295/SegaPCM/C140. This is native evidence/reference coverage, not new
production-MUC projection support. Original source models remain unchanged.

CLI prepares source evidence and independent reference/blueprint before target
conversion. Export publishes flat tracks/*.vgm and blueprint/*.muc/*.midi/*.dat;
extra score voices split into .partNN files. Blueprint preserves score/loop/source
comments and fixed audition defaults; native MUCOM C128/Timer-B, sine bank and flat
SSG E are explicit. PCM is excluded from blueprint. MIDI is a local stdlib SMF
writer. Very slow Timer-B-inexpressible MUC tempo is diagnosed, never clamped.

Important fixes from architectural review: whole gates must be projected before
internal controls. Rounding each pitch slice upward invented false simultaneous
voices. Blueprint now uses complete-duration truncation and the shared original-
device quiet-anchor timing report. Same-grid internal changes settle once; sub-
64th and unknown-pitch intervals have explicit decisions. Native reference remains
source-chip based, with original PCM dependencies/GD3. Optional --score-normalize
retimes a separate reference/conversion input, not original evidence.

Reference and blueprint ownership are independent groups. A failed/skipped group
must not stale-delete the previous other group. Durable pre-mutation backups and
recovery mapping survive failed rollback; all restorations are attempted. Failure
to persist optional musical-layer reports is caught at the CLI seam, permitting
existing MUC routes to continue unless requested score normalization needs the
missing prepared input. Locked-file errors do not justify deleting user products.

Export forwards FM24ms, correction8ms, timeout1200; canonical nativeFM default1ms.
The FM selector reaches native OPNA, projected OPM, MUCOMSX OPLL and SCC-to-OPNA FM.
PSG/SCC state thresholds are not derived from FM. Software curves enabled, bare
opt-out; enhanced native MUC macro search default; legacy macros/loops flags;
score-guided loops remain opt-in and legacy loops suppress them. MGSDRV syntax,
60Hz envelope tables and unrelated compiler switches were not transplanted.

75 focused tests passed before the final shared-anchor adapter, which then passed
16 blueprint/service tests. Small public PSG/OPNA/OPLL exports produced source
products and native-compiled blueprint MUC. PSG replay showed449positive SSG level
writes with tone enabled; this establishes register output, not listening quality.
Authored default-target-unsupported DCSG still published native reference plus
compilable blueprint. No private regression, no broad emulator/catalog sweep.
README unchanged. User owns local_only regression and listening. Full MUCOMSX MUB
runtime regression remains unclaimed.


Final integration: regenerated public PSG/OPNA/OPLL source products after the
quiet-anchor adapter; all three reports passed, each retained15native gates and
15blueprint score notes, and all three MUCOM88 blueprint MUC compiled. Architect
confirmed the complete-duration/anchor finding closed. Optional score-normalized
production input now preserves an enabled source song loop and uses the shared
loop-retiming abstention until validated; --no-song-loop can request first traversal.
15 service/CLI checks passed after that adjustment. MUCOM changes and this note
remain uncommitted; no push. Source products still have independent target status.
