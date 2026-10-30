# Saved regression checkpoint: SCC FM / OPLL rhythm (2026-10-19)

User requested an immediate saved checkpoint at 2% remaining. STOP further
implementation/testing and let the user regress. No Git stage/commit/push.

- MUCOMSX SCC: all five source voices now use existing OPNA FM additive4,
  A/B/C/H/I, with source wave/volume/Segment evidence and shared structure kept.
  SSGExtend/@W native SCC output is removed from this conversion path.
  PSG remains internal MSXD/E/F; optional PSG+SSG layer is unchanged.
- OPLL rhythm: a loop seam inside a prior hit interval now emits a tied rhythm
  continuation, not an invented new attack. Actual seam hits remain attacks.
- When force omits an incompatible carried song loop, rhythm projection now
  excludes attacks outside the emitted first-pass timeline. Full source rows
  and original indices are kept; excluded_attacks records target exclusions.
  Missing timing-map entries report the source sample rather than a bare number.
- OPLL melody preset/custom-patch projection was NOT changed. Default rhythm
  is native MSXG/OPLL; user's command did not enable optional OPNA rhythm G.

Validation: 32 focused conversion/OPLL/SCC/DCSG checks pass. Native MUCOMSX MUB
compilation passed 3/3 authored/public inputs: MIXED5 (PSG3 + SCC5->FM), RHYLOOP
(rhythm loop seam), SCCSRC. See outputs/mucomsx_scc_fm_rhythm_20261019/export.
No private song conversion/listening rerun. Existing F1SPRT3D logs were read only.
Code, README and docs are saved. Native VGM onset/mask verification of tied
rhythm at L remains pending: compilation is NOT evidence of retrigger fidelity.
Direct helper tests cover out-of-range rhythm exclusion with original indices;
a full converter synthetic force-fallback case remains desirable. Architect
review found no confirmed blocker, with this native-onset check still pending.

Next: user regression using a newly started Python process, then inspect its
results. Do not silently resume source OPLL patch work or the suspended enhancer.

---


Private log diagnosis: F1SPRT3D 01/02/05/06/07 fail when a rhythm interval
is split at a loop point without a new hit; 03/04/10 bare integers come from
replay rhythm lookups beyond the shortened first-pass output after loop omission.
No private failed song was re-executed. User owns listening/regression.
