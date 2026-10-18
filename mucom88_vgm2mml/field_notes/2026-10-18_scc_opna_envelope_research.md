# SCC -> OPNA FM+additive envelope research (2026-10-18)

User requested investigation after x68k loop assembly fix, retaining existing
PSG->SSG and preferring fm-additive. Read-only compiler/renderer/docs/official
sources; no probe compile, private conversion or listening. Public design saved
in mucom docs/scc_opna_design.md, checkpoint prepended to handoffs/current.md.
No target implementation or tests added; no Git stage/commit/push.

Official MUCOM88 MML reference: E SSG six-parameter envelope; yTL FM peroperator
control; MT TL softLFO marked notcorrectlyimplemented. Inspected local
.tools/mucomdotnet-research/muc88.cs SETSEV rejects nonSSG except DotNetADPCM
branch; noFM E. Existing mucom_fm.project_state emits held yTL. Proposed SCC
amplituderuns affectcarriers for mainFM/residualadditive without retuning
modulators/newKeyOn. This canexpresscurve but isnot MGSDRV @e table sharing or
verified MUBbyte optimization. HardwareEG curvefit separate lossy futurepolicy.

CH3 independentfrequencies canhelp algorithm7 harmonics, but consumes normalC
and its4operators; not extra4fullFMvoices. StandardS detunes commonnote (OP4,3,1,2,
0..255), no arbitrary4notelines guarantee. Official mucomDotNET MML.txt EXON/EXOF/
EX supports Cpage slots but requiresdistinctDotNet compiler/driverprofile;
currentrenderer rejects CH3special/CSM. Adapter usesDotNetconsole but MUB8/native
compatibility and extensionimplementation areseparate. No phase/Key/fidelity
or performance resultclaimed. Keep specialmode lateroptin.

Ordinary OPNA6FMABCHIJ and PSGSSGDEF leaves atmostoneextra lane after SCC5primary;
mainFM+residual cannot alwaysreserve10. Need explicit quality vs capacity and
sourceomissiondiagnostics. NativeFMmixed competesfurther. ReuseMSXsourceSCCunits/
run/phrase/loop but keepnativewave/Segmentsevidence. YM2151/4M voicebank canseed
search, not certifyYM2608/7987200 output; targetcore/clock/gainhash/pitchmetrics
required. Proposed pipeline nativeSCC->targetOPNAvoicelanes->existingMUC, never
PSGviaOPM. Implementationrequiresusernextdirection.

Sources: https://github.com/onitama/mucom88/wiki/MMLリファレンス
https://github.com/kuma4649/mucomDotNET/blob/master/MML.txt
Local py/mucom_fm.py, docs/mucom_musical_projection.md (CH3 section),
scripts/mucom_compiler.py, installed sourceSETSEV/SETCh3SpecialMode.
