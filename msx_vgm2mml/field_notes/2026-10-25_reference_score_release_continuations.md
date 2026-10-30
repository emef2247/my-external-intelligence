# Reference-score release-continuation audit and implementation

## Current oracle, not stale filename assumptions

The user authorized the eight listed split-score directories as optional test
inputs and emphasized correct authored MML. Focus remained on GRA2_001's six
splits because they already expose the missing interpretation. Read the actual
run_convert_to_sync_blocks.sh and its splitter. The directory has NOW been
regenerated: six numbered MML files plus the master, unlike the earlier98fine
cuts. All six CURRENT same-name MMLs compiled with native MGSC and re-exported
with libkss to VGM byte-identical to their paired source. Direct source-score
comparison is now justified. Proof: outputs/diagnostics/reference_gap_20261025/
current_reference_pairs.json and reference/<stem> artifacts. The older mismatch
note is historical; never keep assuming it after a fixture update.

## Concrete missing interpretation from source-score comparison

Case06 PSG1, local0..40 (master6966..7006): the source uses one observed envelope
with base volume, a16-frame note and4-frame rest while release remains audible.
The converter makes attack[3,19) and tail[19,23) separate notes, then switches
back to the attack envelope at23. Native evidence at19 is a zero-duration fCB
write of the SAME effective tone period1264; volume falls8->7, tone mode and HW
envelope-off remain. At23 the source volume rises to14, independently marking
the next inferred attack. Treating every pitch write as an attack detaches the
release tail and prevents complete-curve/phrase sharing.

A bounded counterfactual motif compiled track usage28->17bytes; definitions
22->26. All180 compared effective volume frames matched. Production authored
16+4 repetitions later passed actual MGSC/libkss at tempos75/225 with unchanged
200-frame effective state and next restart. This is evidence for grouping a
complete observed sounding curve, not recovering a known Key-Off or q command.

## Saved narrow production rule

Under existing --software-volume-curves only, PSG fixed-tone partitions whose
sole boundary reason is pitch_write may join when every ordered row (including
zero-time rows) has identical effective pitch/settings and continuous known time,
volume never rises, and a single audibility gate spans them. Intermediate
low/high-byte pitch excursions, zero-time volume rises, mixer/HW/noise enable,
silence and unknown operations protect boundaries. SCC is excluded because its
phase semantics are different. Original Segments, controls and boundaries stay
intact. Decisions use unchanged_period_write_release_continuation; a separate
unchanged_period_joins counter distinguishes this from inactive reset joins.

Inspected local primary emulator sources in vgmplay/VGMPlay/chips/emu2149.c
(PSG_writeReg0..5 recomputes freq without count/edge reset) and ay8910.c. Web
primary-source fetch failed; no unsupported web claim is made. An authored
local compile against external emu2149 compared8 AY/YM volume-mode/period cases,
12000 samples each, with repeated same-value low/high writes. Audio samples,
tone counters and edges were identical. See period_write_phase_probe/result.json
for source hash. This is emulator-model evidence, not a universal hardware claim.

## Important adverse result: local joins can destroy useful repeats

The first narrow implementation compiled all6 cases but increased PSG1/3 usage
in05 from421/459 to785/785, and06 from429/492 to716/726. It joined some16+4
cycles but rejected otherwise equal cycles because inactive global noise/env
period writes appeared inside them. Partial joining changed note boundaries and
made formerly common structures unequal. More candidate joins, fewer inline
uses and matching effective holds did NOT guarantee less compiled data.

Retain those adverse results under production/ and case05_regression_envelope_
audit.json. Do not report only the good case04 (343/553->178/294) or small
synthetic gains. This is why exact reference patterns and per-channel used-byte
comparison matter more than candidate counts or padded MGS size.

The next saved correction allows known global wNC/ePL/evM controls through the
scan only while ALL rows remain fixed-tone/HW-off. Inactive noise/period latch
updates do not change that channel's effective sounding state; latches remain
in native controls for later or other-channel use. Paired tests retain a barrier
for transient mode3/HW-on. Architect approved this as effective-state reasoning,
not fixture-specific tuning. Six-case revalidation of this final correction is
in progress under production_effective_controls/. If adverse buffer effects
remain, do not declare the rule a completed compression improvement.

## Remaining missing abstractions

Absolute-volume variants consume separate envelope entries even when one
relative shape plus base volume would suffice in source MML. Source release-phase
and relative curve-family recovery are distinct from the implemented observed
continuation. Existing long-observed-curve selection and macro policies remain
unchanged. Do not invent release tails or silently replace the allocator.

README remains user-owned and unchanged. Other seven authorized directories
were inventoried only, not broadly converted. No fixtures changed; no private
music copied into public docs. No stage/commit/push.

## Final bounded validation and remaining shared-bank tradeoff

The effective-control correction was revalidated on all six CURRENT paired
GRA2_001 cases; all canonical conversions and actual MGSC compiles succeeded.
PSG1/3 used-byte pairs, previous reset-only -> final:
01:123/141->80/106;02:368/375->149/153;03:454/449->86/85;
04:343/553->179/286;05:421/459->341/343;06:429/492->171/179.
These are per-track compiler used bytes, not MML or padded MGS sizes.
Final same-period joins01..06:11,80,42,84,97,122. Native effective hold error and
projected boundary shift remain zero in the recorded source/projection scope.

Capacity is still NOT monotonic across all chips: case05 MGSC track4(SCC1)
1261->1790 and sum of all track used bytes10100->10446. Shared32-envelope
selection couples chip outcomes; existing long-first policy was not changed.
This is a remaining allocator/relative-family problem, not evidence that the
PSG grouping rule changes native volume. Preserve and report this adverse
result rather than advertising a universal buffer fix.

Authored tests include full ordered intermediate pitch/volume/mixer/HW guards,
inactive-noise/period updates vs transient enable, SCC exclusion and real
75/225 MGSDRV/libkss repeated20-frame release curves.55 related test methods
passed (23 projection,13 ties,7 loop-first,10 existing envelopes,1 inactive-reset
actual playback,1 unchanged-period actual playback). Latest authored check
includes alternating inactive wNC/ePL/evM writes and exact200-frame state/onsets.

Real paired oracle check: case06 final MGS->VGM compared with input VGM on
PSG1/3 for first200 relative60Hz frames, amplitude+enable exact, mismatch0.
Each channel aligns at its first positive enabled amplitude write; raw sample
transition offsets are retained. This is a bounded prefix validation, not full
song/all-chip phase equivalence. Evidence:case06_oracle_replay/comparison.json.

Japanese user-facing analysis is saved at outputs/diagnostics/
reference_gap_20261025/analysis_ja.md. Current references and all bad/intermediate/
final candidates remain locally inspectable; no fixture was overwritten.
README untouched. No new CLI switch: production additions remain under
--software-volume-curves, current default off, explicit --no- alternative.
Architect reviewed the narrow guard code and found no material blocker.

Next useful work: exact base-volume-offset envelope families, followed by a
controlled shared-bank/per-channel capacity selection comparison. Do not
introduce that policy silently, invent release tails or assert original q/score
recovery. Other7 directories need conversion only if they add missing coverage.
