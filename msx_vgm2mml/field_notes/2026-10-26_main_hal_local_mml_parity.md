# Main-to-HAL ordinary MGSDRV MML parity

User requested committing current work before comparing ordinary MGS MML to
main, one local_only folder each for PSG/PSG+SCC/OPLL. Implementation commit9da7011
contains HuC/shared-HAL/blueprint; related private notes committed as3264937.
The baseline is local mainab58c3e. Fixed archived runtime snapshots were used;
no implementation changed to obtain parity. README stayed user-owned/unchanged.

## Method and scope

PSG: psg/vgmrips.net/TWINBEE,7 inputs.
PSG+SCC: psg_scc/msxplay.com/gra2_msx/gra2_001/with_sync_mark,6 inputs.
OPLL: opll/www.smspower.org/MSLND,10 inputs.
Canonical settings on both: normalize-lengths, software-volume-curves,
fm-state-ms24, normalization-ms8, force, dump-passes, default loops/macros.
Compare ordinary root <stem>.mml (exclude blueprint), then compile both with
the same MGSC and compare MGS. Preserve logs, commands, native/intermediate
pass dumps and hashes under outputs/diagnostics/main_vs_musical_hal_9da7011.

## Results

All23 MML pairs byte-identical:7/7,6/6,10/10. Both versions compile all23 inputs
successfully; all23 MGS pairs byte-identical. Input hashes unchanged; no source
or reference MML modified. MSLND09 has10 time-zero DCSG writes omitted by BOTH
versions under force; all other22 inputs have0 omissions. This is selected
ordinary-output compatibility, not a new hardware or source-score proof.
No broad private regression or hardware listening performed.

## Practical lessons

Generated MML may contain CP932 Japanese titles. Hash bytes first; UTF8-only
text decoding fails despite successful conversion/compilation. The audit uses
UTF8 thenCP932 decoding only for optional text/diff inspection. Exact equality
does not depend on lossy comment/whitespace normalization.
Snapshot only runtime code: duplicating large fixture trees onto mounted Windows
storage wastes preparation time. Zero and unavailable omission counts must not
be conflated, and both versions' omission details must be checked even when MML
matches. Here the nonzero omission was a real shared force-policy result, not
a new regression. Public handoff records the audit; generated/private material
stays untracked. Commit this note separately; do not push the private repository.
