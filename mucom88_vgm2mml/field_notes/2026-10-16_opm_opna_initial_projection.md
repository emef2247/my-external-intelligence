# Independent OPM -> OPNA basic FM checkpoint (2026-10-16)

User authorized roadmap step1. Preserve source OPM State/Segments and PCM IR;
independent py/opm_opna.py creates explicit OPNA target evidence, then existing
MUC whole-gate/normalization/structure/rendering. No vgm-conv dependency/code.
FITOM static voice references informed inspection only; do not transfer its
carrier/velocity normalization or HW-LFO omissions as source-faithful policy.

Implemented basic operators, algorithm/feedback, KC/KF nominal pitch, partial
Key and held edits; fixed whole-song active source-ch allocation to6 FM parts.
Default >6 error even with force; explicit --opm-channel-policy omit keeps first6
and diagnoses all omitted channels. OPM source occurrence IDs, original raw IDs,
time/channel/register and target ID/sequence are separate in mapping CSV.
Target startup mode/LFO/SSG-EG init serialized because normalization replays
from fresh OpnaRegisterState. Key mask logical order is not register-bank order;
FNUM high always precedes every low commit (shared latch must not be cached).
Target analysis clock7987200Hz prevents double source-clock correction.
Native OPNA SSG/PCM uses its native clock independently of projected OPM FM.

Force removes DT2, neutralizes effective LFO/CSM; ch7/C2 noise operator TL mute;
pan0 centered/all-operator TL127 approximation. These are explicit target losses,
not physical/phase/release fidelity. DT1/EG physical rates remain uncorrected.
Pitch outside OPNA global FNUM/BLOCK range errors normally; force clamps with
source/target Hz and cents. Missing KC/KF reset0 assumptions remain target-only.
Native OPNA FM wins mixed FM conflict under force, omitted OPM reported.

Authored tests:6 OPM route +3 force +3 CLI =12 passed. Initial force assertion
used nonexistent m1_tl/c2_tl CSV names; corrected only test to op1_tl/op4_tl.
Authored public SEGAONE through actual batch --muc-only, force, explicit channel
policy and best-effort generated MUC A/K tracks plus voice/PCM bank. Mapping41
writes,1 FM gate, nominal pitch max error0.13969820557162654cent. Artifacts in
outputs/diagnostics/opm_opna_check_20261016; source/target IDs, state/gates and
MUC inspected. Synthetic MSM loop checks shared2traversal/end4000/L2000.
No private conversion, compiler run, listening or broad regression. User owns it.

Architect reviewed boundary before implementation and read-only saved checkpoint:
no basic-route blocker; cautions about omit-without-force visibility and extreme
pitch range were fixed with explicit reporting and focused tests.
Only working-tree changes; no stage/commit/push/branch changes. Other existing
private-note changes preserved. Next user regression, then roadmap step2 x68k
PSG/SSG interpretation reuse; no wholesale FM rewrite justified by size failures.
