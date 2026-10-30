# PDX byte-budget user regression and completion (2026-10-22)

User resumed work and supplied GFII.log, ABII.log, POWRDF.log and GLPC.log.
Log results were matched against the existing outputs/*/results.csv failure
details. No private VGM reconversion, replay, listening or local_only tests
were run. Successful export is not a listening/native playback result.

| Set | Export success | PDX measured | Maximum PDX bytes |
|---|---:|---:|---:|
| GFII | 10/10 | 8 | 1174924 |
| ABII | 4/9 | 9 | 1197616 |
| POWRDF | 6/9 | 8 | 1164260 |
| GLOC | 15/17 | 16 | 960156 |

All41 PDX with measured file sizes fit the table-inclusive1,200,000-byte
ceiling. Four inputs have no measured PDX; do not count those as verified
PCM outputs. GLPC.log contains the GLOC set, not a separate GLPC source.

GFII04 plannedPDX1362412->1174924B;GFII06 planned1281216->1156224B.
Both exported successfully; all10GFII exported. Additional byte-budget
replacements occur in ABII02/05/07/08/09 and POWRDF03/04. Their PDX fit,
but successful PCM packaging does not guarantee the MDX compiler can finish.

## Remaining failures (existing reports, not reproduced)

- ABII / AFBII02.vgm: MDX capacity exceeded; generated MML and PDX are retained: data inconsistency: MDX track 5 offset exceeds the maximum 0xfffe; PDX 1153436B.
- ABII / AFBII05.vgm: MDX capacity exceeded; generated MML and PDX are retained: data inconsistency: MDX track 5 offset exceeds the maximum 0xfffe; PDX 1141320B.
- ABII / AFBII07.vgm: MDX capacity exceeded; generated MML and PDX are retained: data inconsistency: MDX track 5 offset exceeds the maximum 0xfffe; PDX 1197616B.
- ABII / AFBII08.vgm: MDX capacity exceeded; generated MML and PDX are retained: data inconsistency: MDX track 2 offset exceeds the maximum 0xfffe; PDX 1171080B.
- ABII / AFBII09.vgm: MDX capacity exceeded; generated MML and PDX are retained: data inconsistency: MDX track 4 offset exceeds the maximum 0xfffe; PDX 1140204B.
- POWRDF / POWRDF03.vgm: MDX capacity exceeded; generated MML and PDX are retained: data inconsistency: MDX track 3 offset exceeds the maximum 0xfffe; PDX 1164260B.
- POWRDF / POWRDF04.vgm: MDX capacity exceeded; generated MML and PDX are retained: data inconsistency: MDX track 2 offset exceeds the maximum 0xfffe; PDX 1147138B.
- POWRDF / POWRDF07.vgm: MDX capacity exceeded; generated MML and PDX are retained: data inconsistency: MDX track 8 offset exceeds the maximum 0xfffe; PDX 996128B.
- GLOC / GLOC09.vgm: MDX capacity exceeded; generated MML and PDX are retained: data inconsistency: MDX track 6 offset exceeds the maximum 0xfffe; PDX 494976B.
- GLOC / GLOC13.vgm: MDX capacity exceeded; generated MML and PDX are retained: data inconsistency: MDX track 7 offset exceeds the maximum 0xfffe; PDX 651864B.

All10 failures are MDX track-start offsets exceeding0xfffe; none is a
PDX byte-cap overflow. Existing exporter labels them pcm_projection_blocked
because typed PCM MDX construction failed after MML/PDX generation. Do not
misinterpret that status as discarded PCM or a remaining PDX size failure.
No MDX-size workaround or skip was added; expanding that scope requires a
separate decision. The256KiB player allocation does not remove this current
compiler/offset restriction.

## Completion review and verification record

Architect final read-only review completed after the prior usage-limit
interruption: no material blocker in the byte-budget implementation. Checked
source-only preflight flag stays on its copy; final binding/actual-file guards
remain active; onset repair remains within bytes; force capacity omissions
occupy no voice; explicit quality modes stay explicit; ownership protection
precedes the public writer. No new tests or private conversion from review.

Before interruption,253 public/authored checks passed. A subsequent additional
unused-parallel-source-assets check was added, and the byte-capacity file
passed8/8. Thus254 distinct focused cases were checked across those runs.
No broad test rerun on resume: implementation unchanged and results retained.
PublicSega4/4 and the authored20asset->18rep/20event0omission MML/MDX/PDX
probe remain the output/intermediate evidence recorded in the preceding note.

Aggregate artifact:outputs/diagnostics/pdx_byte_budget_20261021/
user_regression_20261022.json. Original logs and source material remain private.
No Git staging, commit or push in either repository.
