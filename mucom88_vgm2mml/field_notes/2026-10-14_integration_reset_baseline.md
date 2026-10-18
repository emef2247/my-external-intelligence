# 2026-10-14: x68k inheritance integration reset

User replaced py/ with current sibling x68k_vgm2mml and retained py.org.
Canonical frontend and shared scripts/tests/public baseline dependencies/source
reference docs now follow source revision 330a6fdb9a0e4e19e57321428b01b144524cbdf6.
Content hashes and exclusions are in public docs/integration_baseline.json.

Current production output is inherited MDX/MGS, not musical MUC/MUB. Preserve
MUCOM design principles while reconnecting musical interpretation/structure
before YM2608 projection. Do not restore the former per-Segment MUC projection
merely to make export work. It remains a local evidence archive with its old
frontend/exporter/native-MUB comparator/three target tests. User's py.org and
README.mucom88.md are untouched. Authored fixture generator/hash test and native
compiler adapter remain active. No private fixtures/tools/generated outputs copied.

WSL help works; OPNA from_fm/3ch_test emits MDX MML and 48 intermediate CSVs.
Public PCM fixture emits 8 native ADPCM-B spans, 4 PDX samples, MDX/PDX via
best-effort. This is inherited X68000 target evidence, not native MUCOM output.
Source/state/Segment/PCM binding diagnostics were inspected.

Final reset suite: 797 tests, failures=9, errors=11, skipped=81. All failure groups reproduced in source. MGS 134-case baseline and public MUCOM fixture checks
pass. Four failures already reproduced in source: normalization adopted flag,
legacy silent-track allocation, stale unsupported-source expectation, OPLL clock
fixture for MGS. Duration/relative replay test helpers lack source_clock_hz;
one representative from each group also reproduced in source. Preserve baseline
rather than reshape source semantics to conceal inherited stale-test failures.

No stage/commit/push. Existing unrelated private note changes are preserved.


After this reset, native musical MUC connection was implemented; current status
is in 2026-10-14_native_muc_gate_integration.md and the public handoff. Above MDX
status describes the earlier reset checkpoint only. The reset SHA256 record is
historical, not an assertion of current content identity.
