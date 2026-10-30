# User DCSG x2 regression — 2026-10-20

Record-only request at 1% remaining allowance. No code changes, reconversion,
tests or listening. User identifies DCSG as a new source path absent from
msx_vgm2mml. Results are user-reported, not independently reproduced. The exact
code revision used by this run was not supplied; do not attribute the result to
a particular macro/state-spelling checkpoint without its revision/artifacts.

Command:
```sh
python scripts/export_mucom.py tests/fixtures/local_only/2xSN76489A/vgmrips.net/MSLANDAC/ --outdir outputs/2xSN76489A/vgmrips.net/MSLANDAC/ --target mucomsx --force
```

## Results

20 entries: 11 success, 9 conversion_failed. Every reported failure is MUC
source input capacity exceeded (limit 65536 bytes). Compiler DATA, PCM capacity,
playback and audio fidelity are not established by this report.

| Input | Status | MUC bytes | Diagnostic run |
|---|---|---:|---|
| MSLDAC01.vgm | conversion_failed | 94096 | run__fl9moi6 |
| MSLDAC02.vgm | conversion_failed | 128882 | run_gi28c_xw |
| MSLDAC03.vgm | success | | |
| MSLDAC04.vgm | success | | |
| MSLDAC05.vgm | conversion_failed | 139773 | run_4adf28bu |
| MSLDAC06.vgm | success | | |
| MSLDAC07.vgm | success | | |
| MSLDAC08.vgm | conversion_failed | 71307 | run_weo2bguo |
| MSLDAC09.vgm | success | | |
| MSLDAC10.vgm | conversion_failed | 152357 | run_sejjgzyx |
| MSLDAC11.vgm | success | | |
| MSLDAC12.vgm | success | | |
| MSLDAC13.vgm | conversion_failed | 210784 | run_y6qm2m_5 |
| MSLDAC14.vgm | conversion_failed | 179120 | run_e4r_pc6v |
| MSLDAC15.vgm | conversion_failed | 97643 | run_uawna1m3 |
| MSLDAC16.vgm | success | | |
| MSLDAC17.vgm | success | | |
| MSLDAC18.vgm | success | | |
| MSLDAC19.vgm | conversion_failed | 82486 | run_1latnlhb |
| MSLDAC20.vgm | success | | |

Each failure report is under:
`outputs/2xSN76489A/vgmrips.net/MSLANDAC/_diagnostics/<stem>/<run>/report.txt`.
Source fixtures remain private. No fixture contents copied into public docs.

## Current code routing (read-only check)

DCSG parsing/state interpretation is new. dcsg_music constructs derived AY/PSG
views, then uses shared MSX extract_notes and pre_envelope_loops.prepare. The
MUCOMSX renderer routes those notes through the same PSG state spelling and
structural source-plan projection as AY PSG. Both native MSX PSG parts and the
second chip's OPNA SSG parts use that shared route. Final compact_tracks uses
the shared MUCOMSX macro adapter: atomic baseline plus four MSX width/rank
allocations, up to 32 macros and 512-character searched bodies, with exact
expansion checks. This is not a separate legacy DCSG macro algorithm, nor a
claim that MGSDRV arbitrary software-envelope tables are available in MUCOMSX.

Connection does not prove that each musical repetition becomes a finite MUC
loop: projected duration/control equality and target limits still apply.
No new diagnosis of the reported size failures was attempted in this turn.
