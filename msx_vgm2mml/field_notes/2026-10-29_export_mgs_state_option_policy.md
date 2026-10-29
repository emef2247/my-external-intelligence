# 2026-10-29: simplified export_mgs state and macro switches

User requested replacing fm-state-ms, psg-state-ms and scc-state-ms on
export_mgs.py with one state-ms setting. Default remains24ms; its value is
forwarded to all three existing lower-level parameters. Unlike the previous
export defaults, PSG/SCC bounded volume-curve approximation is now requested.
The user explicitly accepted coupling the untested PSG/SCC thresholds here;
this supersedes the earlier opt-in frontend policy, not source preservation or
algorithm protections. No shared-algorithm equivalence or listening validation
is claimed. Original samples, gate boundaries and protected effects remain.

The exporter now supplies internal native-register error limits: maximum1,
weighted mean0.25. They use the previously documented experimental setting as
an initial export policy, not a chip specification, calibrated amplitude or a
listening-established threshold. When a fitted clock is applied, the existing
post-clock-error safeguard still abstains from curve approximation.

Removed from export_mgs.py: individual state/error flags, normalize-lengths,
enhance-rhythm/enhance-drums, and PSG solo selection with underscore aliases.
Core converter, batch and VGM exporter options remain available. Internal
settings are normalize_lengths=False(thresholds still request normalization),
enhance_drums=False and psg_part=all. legacy-macros is now a bare opt-in flag;
enhanced macros remain default, with a hidden bare enhance-macros compatibility
alias. Software-volume-curves switch and its optional boolean are unchanged.
No unrelated flags were removed, and README remains user-maintained.

Validation:66 affected export/configuration/guide/score tests passed. An
additional real public PSG export inspected curve_approx.json: enabled24ms,
limits1/.25, native segments and note boundaries unchanged. CLI help/forwarding,
removed switches, invalid finite/positive thresholds, no-BOOL legacy behavior,
blueprint-only isolation and public MGS compilation were checked. No local_only
regression or listening run. Public and private changes remain uncommitted.

## Follow-up: software volume switch is also value-free

User requested removing BOOL from no-software-volume-curves as well. It is now
a store_false flag with defaultTrue: absent enables software-volume-curves;
present disables it. Hidden positive compatibility alias is store_true without
a value. Removed the obsolete boolean parser and argument preprocessing. Tests
cover both states, input names such as false, help without BOOL and rejection
of separate/equals-form boolean arguments. README unchanged.
