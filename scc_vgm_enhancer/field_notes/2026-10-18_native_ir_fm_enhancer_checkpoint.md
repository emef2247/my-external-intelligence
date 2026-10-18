# Native IR and SCC FM enhancer checkpoint

Implemented the approved VGM -> State/actions/Segments -> VGM pipeline in the
separate scc_vgm_enhancer repository. Original source bytes are not used as an
output shortcut. Typed ordered actions supplement interval State and musical
Segments for trigger/memory semantics. Native PSG/SCC output is generated before
optional independent OPM/OPNA layers and remains available on FM failure.

Twenty public authored tests passed. Canonical batch conversion with both FM
targets succeeded for MIX, LOOP and PSG (3/3). Independent authored schedule
checks compared 190 / 332 / 12 native writes, respectively. LOOP retained sample
11025 as its anchor and 33075 as its end; native schedules also matched across
two traversals. No private conversion, listening or hardware check was run.

Real-core probes use external BSD-3-Clause ymfm pinned at
81aec25ccbb98f4873a255f7551ac4dadac59b4a. Separate chip/clock/calibration identities
are part of fitting. Physical OPNA pitch was checked at native output rate and
48kHz, establishing the clock*FNUM*2**block/(144*2**21) convention with MUL=1 in
this implementation. Do not backport this into nominal sibling formulas without
checking their octave/block conventions. Audio tests covered 110/440/880/1760Hz,
operator mapping, calibration and simultaneous voices. Public sine/square fits
preferred evaluated additive fallbacks; this is not proof of FM superiority.

Default projection selects SCC parts 1-4 and preserves all five native parts.
This is provisional pending the user's optional four-part-policy clarification;
--scc-parts all is available. User owns private regression and commits. No Git
stage/commit/push occurred. See the public handoff and pipeline documentation for
current commands, pass artifacts and explicit source/loop rejection limits.
