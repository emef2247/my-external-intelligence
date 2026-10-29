# SALAMD six-way saved macro/loop comparison — 2026-10-30

All14songs in6current modes are complete, plus test2v1.1.0 baseline. Read only
saved MML/MGS/diagnostics; no private conversion, compilation or listening.
Staged input hashes match across all7sets. Effective command flags and PSG/SCC
stateNone/curveNone, FM24ms, correction8ms, software curves on, score-normalize
off were verified. User's folder LEGOCY spelling is intentional and read as-is.

Mode                  success MML14bytes MGScommon10 macros14
v1.1.0                 12/14    182367       76160      410
enhanced+normal        10/14    191799       79744      393
enhanced+legacyloops   10/14    192514       81792      395
enhanced+guided        10/14    190224       76800      392
legacy+normal          12/14    219313       77184      407
legacy+legacyloops     12/14    222183       79104      411
legacy+guided          12/14    217065       74496      407

Alllegacy modes fail only02/06, matching baseline. Allenhanced modes alsofail
08/11. Paired same-code macro switch recovers08/11 in allthree loop policies.
02failsch1,06ch2,08/11ch5. Normal/guided #alloc ch5 matches baseline3685/6489,
so decreased allocation is not an explanation for their new failure.

Enhanced reduces source text and macro definitions, but legacy compiles more
songs and uses fewer bytes on common successes. Legacy+guided is best current
aggregate. On the same12successful songs, baseline100992 vslegacy-normal102528,
legacy-legacyloops104448,legacy-guided99840: guided saves1152bytes(1.14%). On
common10, guided saves1664bytes(2.18%). Not uniformly better:08is10496->12032,
11is14336->13312. No current successful MGS SHA matches baseline. No listening
or full musical equivalence claim follows from file sizes/counts.

Source integration order: merged MML/#alloc first, then late enhance_macros
normalization/phrase reselection, then final formatting. Allocation is not
recomputed after enhanced commands change. This is consistent with capacity
regression, not proof of each byte-level cause. Retained macro flag skips the
late enhancer; it does not restore all historical shared preprocessing.

Standing user authorization was to choose macro default after comparing results.
Decision: export_mgs defaults is_legacy_macros=True; --enhance-macros visible
opt-in. --legacy-macros hidden compatibility remains accepted as a bare flag.
Normal-loop default unchanged; --score-guided-loops recommended for this dataset
but remains opt-in. Canonical/batch defaults and MUCOM defaults not changed based
on MGS-specific measurements. No README change, commit or push. Existing export
CLI tests updated for default/opt-in/end-of-options;10passed including a small
public conversion. No additional tests or private sweeps were needed.

Ignored evidence: outputs/diagnostics/salamd_six_way_20261030/ contains Japanese
analysis,98measurement rows,paired macro comparisons,channel allocations,
effective options,hashes and summary. Previous missing-output/waiting notes are
superseded. No private MML passages copied into notes. User owns listening and
further local regression. Investigate enhanced compiled capacity before trying
to minimize text/macro count further; avoid silently growing buffers or revising
source gates merely to recover compilation.


## Per-song measurements retained from the Japanese report

Each cell is ordinary MML file bytes / compiled MGS file bytes. MML includes
layout/comments; MGS file size is not a per-channel buffer or fidelity measure.
The source report's SALAMD09 enhanced-guided MML cell contains a visible typo;
its numeric measurement is 6,940 bytes. No conversions were run for this record.

| Song | v1.1.0 | Enhanced normal | Enhanced old loops | Enhanced guided | Legacy normal | Legacy old loops | Legacy guided |
|---|---|---|---|---|---|---|---|
| SALAMD01 | 12,373 / 11,264 | 13,485 / 12,288 | 13,652 / 12,416 | 12,263 / 9,472 | 17,829 / 11,648 | 18,417 / 11,520 | 15,983 / 8,704 |
| SALAMD02 | 23,799 / failed | 22,952 / failed | 21,698 / failed | 22,751 / failed | 26,035 / failed | 25,174 / failed | 25,761 / failed |
| SALAMD03 | 15,028 / 10,368 | 15,467 / 11,264 | 15,086 / 10,880 | 15,437 / 11,264 | 16,873 / 11,008 | 16,599 / 10,624 | 16,917 / 11,008 |
| SALAMD04 | 8,817 / 5,248 | 8,884 / 5,120 | 9,214 / 5,376 | 8,940 / 5,120 | 9,887 / 5,248 | 10,160 / 5,376 | 9,918 / 5,248 |
| SALAMD05 | 15,392 / 11,008 | 15,282 / 10,880 | 15,333 / 11,264 | 15,138 / 10,880 | 16,644 / 10,496 | 17,108 / 11,008 | 16,642 / 10,624 |
| SALAMD06 | 21,722 / failed | 23,496 / failed | 23,679 / failed | 23,473 / failed | 27,092 / failed | 27,206 / failed | 26,989 / failed |
| SALAMD07 | 13,502 / 11,136 | 15,695 / 12,416 | 15,654 / 12,544 | 15,695 / 12,416 | 17,879 / 12,032 | 17,913 / 12,160 | 17,879 / 12,032 |
| SALAMD08 | 13,147 / 10,496 | 14,422 / failed | 14,516 / failed | 14,422 / failed | 17,000 / 12,032 | 16,988 / 12,032 | 17,000 / 12,032 |
| SALAMD09 | 6,541 / 4,480 | 6,843 / 4,736 | 7,503 / 4,992 | 6,940 / 4,736 | 8,189 / 4,608 | 9,024 / 4,864 | 8,185 / 4,608 |
| SALAMD10 | 5,869 / 3,328 | 6,175 / 4,096 | 6,185 / 4,096 | 6,175 / 4,096 | 6,341 / 3,840 | 6,331 / 3,840 | 6,341 / 3,840 |
| SALAMD11 | 16,933 / 14,336 | 17,333 / failed | 17,222 / failed | 17,333 / failed | 21,785 / 13,312 | 21,801 / 13,312 | 21,785 / 13,312 |
| SALAMD12 | 10,996 / 8,448 | 13,175 / 7,296 | 13,312 / 7,424 | 13,041 / 7,168 | 13,184 / 7,296 | 13,354 / 7,424 | 13,050 / 7,168 |
| SALAMD13 | 14,483 / 9,344 | 14,989 / 10,240 | 15,857 / 11,392 | 15,015 / 10,240 | 16,974 / 9,600 | 18,505 / 10,880 | 17,014 / 9,856 |
| SALAMD14 | 3,765 / 1,536 | 3,601 / 1,408 | 3,603 / 1,408 | 3,601 / 1,408 | 3,601 / 1,408 | 3,603 / 1,408 | 3,601 / 1,408 |
