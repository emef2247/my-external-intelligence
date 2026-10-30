# Native SCC5 projection — 2026-10-29

User requests native SCC as default within MUCOMSX, same musical-content intent
as MSX/MGSDRV for PSG+SCC inputs. User's screenshot of author response confirms
real-MSX support is still being prepared. Current native profile is Playground/
MDPlayer4Mac compatible MUC generation; user performs listening/local regression.
No new installed compiler/runtime or MUB container compatibility is claimed.

New mucomsx_scc_native adapter emits exact signed32sample ?WD waves (global
content dedup,64IDs), ordinary C128/arbitrary-clock rows, native SCC pitch table
plus absolute direct period offset D0 ?DBdelta,1, flat E and held linear v runs.
It does not fit guessed ?EP curves, rewrite source Segments, or use FM additive
projection. Period+1/source-clock scaling is explicit; target clamp diagnosed.
All4087sounding source periods9..4095 reconstruct at nominal1789773Hz in tests.
Source clock1789772 differs minutely; register rounding can be unavoidable.

Routing: PSG->MSXD/E/F, SCC0..4->SCCA..SCCE. Prior additive4route is explicit
--scc-output opna-fm. Global target stilldefaults mucom; --target mucomsx is needed.
FM-state affects compatibility FM/OPLL, not nativeSCC. Shared performance and
volume-curve engine hashes match currentMSX exactly. MGSDRV envelope syntax,
macros and clocks are not pasted into MUC. Two-level native repeats are enforced
before macro extraction; exact expandedtokens remain identical. Source planner
already caps repeatcounts255. Added ?DB atom to native macro lexer.

NativeSCC no longer inherits the old DOS64KiB source ceiling as a generation
failure; emits capacitywarning and MUC.64wavepool and1023physical-line bounds are
still explicit. No arbitrary wave replacement or automatic ID reassignment.
Wave CSV, source notes, target units, generic native pitch fields and JSON routing
are inspectable. Legacy projected_opna_period CSV column remains with native SCC
period values for compatibility; target_period/target_chip disambiguate it.

93 focused tests passed after the final short-partition policy fix. Public psg_scc001 and002 MUC-only exports completed.
001: two activePSG parts, two activeSCCparts, two waves,16584byteMUC.
Architect identified that to_notes adapts score partitions, not necessarily
complete audibility gates. The prior blanket <=8ms policy labeled1..11sample
register-transition slices as whole notes and removed167PSG+9SCC partitions.
The new mixed native SCC profile now bypasses that deletion for PSG/SCC, retaining
all743 interpreted sounding partitions in the target. Positive-interval timing
allocation still applies. No automatic tying or reinterpretation of attacks was
introduced. Other profiles retain their existing policy, with corrected diagnostic
wording. Added3sample mixed-source regression. Do not advertise playback
 equivalence solely from generated text, source counts or tokenidentity.
All original sample evidence remains. Source native VGM and MUCOM88 blueprint
are independent of the production native SCC target. User owns local_only tests.

No README changes, commit or push. Preserve earlier uncommitted port/removal/
curvebackport work and unrelated -w.mub. Runtime/SCC+ hardware waveform-sharing
policy must be validated with the author's future MSX compiler before claiming
real hardware compatibility. MUC-only regression command is documented publicly.

Final public002 rerun also retained all204 interpreted sounding partitions with
zero omissions. Both public datasets passed original-to-target partition checks;
this is intermediate structural evidence, not listening or compiler equivalence.
