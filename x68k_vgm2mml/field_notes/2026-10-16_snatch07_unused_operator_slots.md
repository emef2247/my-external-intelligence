# SNATCH07 unknown-register rejection (2026-10-16)

## Scope

Read existing user-run results.csv, conversion diagnostics, projection guard
and native register classification. Did not reconvert, compile, run tests or
change conversion code. Source-only command census used command_times with one
original stream pass; no Segments or waveform were regenerated.

## Findings

Failure: OPNA FM projection encountered an unknown register effect.
In both port0 and port1, registers83/87/8B each receive FF once at sample0.
Six original-pass writes in total. All are classified uninterpreted, which
causes the broad unknown-effect guard to reject the source.

These are holes in the per-operator SL/RR register address layout: the local
channel index encoded in the low two bits is3, while valid operator channel
indices are0,1,2 per port. Existing register_channel already returns no channel.

The ymfm OPNA operator_offset mapping skips low-two-bit3 offsets, and sustain/
release accessors address base80 plus those mapped offsets:
https://github.com/aaronsgiles/ymfm/blob/main/src/ymfm_opn.h
Mapping checked on2026-10-16. This is implementation-reference evidence, not an
independent hardware measurement.

This differs from an active unsupported synthesis feature. Suitable follow-up:
retain native write evidence with an explicit unused-operator-slot category;
do not reject only this narrowly recognized address family in target projection.
Do not whitelist arbitrary uninterpreted controls, test registers, CH3, CSM or
change source state to invent a sound effect. No such fix was implemented here.

The currently scoped --force applies only to unsupported SSG intervals and
therefore cannot bypass this earlier FM guard. Later conversion stages have
not been tested after removing this first blocker.

The pasted command repeats --outdir; argparse uses the final SNATCHER output
path, matching saved results. It is not the cause of this projection failure.
