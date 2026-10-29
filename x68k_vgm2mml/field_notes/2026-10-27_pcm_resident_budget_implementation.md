# Configurable PCM resident budget implemented (2026-10-27)

User authorized implementing the prior capacity proposal. This milestone is the
shared resident-budget stage, not automatic low-rate retries. Current1.2M file
cap/fidelity defaults retained. No private regression, player/config change,
staging/commit/push. Preserve all earlier uncommitted MSM6295 work.

PcmCapacityPolicy frozen in py/pcm_capacity.py carries optional resident bytes,
loader direct(0) or mxctrl(270), configured margin4096 default. Effective PDX
cap=min(1200000,buffer-overhead-margin); includes768-byte table. CLI/core expose
--pcm-buffer-kib N, --pcm-loader direct|mxctrl,--pcm-buffer-margin-bytes N.
Explicit312KiB+MXCTRL =>315122B file budget. No assertion this is actual MADRV
allocation or hardware-safe. Nondefault margin without buffer rejects rather
than silently ignored. Constructor validates integers/nonnegative reserve and
budget belowtable. Legacy absentbuffer keeps1.2M/unconfigured.

Same resolved policy threaded canonical/export/listening routes, nativeOPM and
PSG adapter, OPNA forwarded options, target preflight, reducer/representative
repair, force, parallel lanes, normalization preflight, binding and packed-file
check. Projection retains policy so finalpacker cannot revert to defaultcap.
All candidates sourcepreserved; unused/mergedtarget view only. Finalactualfile
check includes additionalpadding; reject removes oversizedgeneratedPDX. Separate
sample-format,slot and eightvoice limits unchanged. Strict no silentloss.

Timing/format/sourcevalidation/merge/conversion reports retain profile; stats
reconstruct policy from requestedcanonical config first, then timing/merge/
validation, never trust storedfit conclusions. Onlyowned/currentbinary makes
actualsize/transfer measured. Emptytarget no transfer. Runtime unmeasured even
when configuredcapacity passes. Native decoded/player workspace not included.
PSGcontinuousmix remains outside approximatebanks; finalguardstill applies.

Worker scoped reducer tests62, stats23; main256 relevant authored/public tests
pass end-to-end, including currentMSM6295/C140 routes. Architect review found no
blockingbypass; requested tests forparallelforce andnormalization added/pass,
minor ineffective marginwithoutbuffer fixed. Two existing multitracktests also
failed against HEAD pcm_mdx: they assumed65535 per-sample cap despiteEXPDXdefault.
Now explicitly standardformat in those two tests; keepEXPDXdefaultunchanged.

Ignored outputs/diagnostics/pcm_resident_budget_20261027/ stores tests.log,
baseline_multitrack.log and HEADmodule reproduction, authoredfixture/legacy/
small/ pass evidence. Arithmetic originalfixture yields legacy2samples/2events
PDX1280B; buffer1KiB margin0 =>1sample/2events PDX1024B, oneexplicitforcedmapping.
Originalinput hash and11nativeevidence CSVs unchanged. MappingCSV inspected:
event0 staysasset0, event1 mapsasset1->0, sourceonsets0/1445,end2890, rate15625
preserved; reported gain/headroom/timbreerror remains explicit. No acousticclaim.
Docs/pcm_playback_budget.md explains options; READMEuntouched. Handoff updated.
Next:user playbackregression using actual resident/loader. Automatic profile
candidate comparison and phase-safePSGbank extraction remain distinct later
work. Do not equate capacity arithmetic with full RAM or playback verification.
