# FM pan unknown misreported as mute (2026-10-15)

User supplied finite600s regression: RTYPE15 succeeds; MISTYB01..03 fail
before MUC emission at the hardcoded FM pan0 guard. Read the supplied existing
reports and normalized State/music CSVs only. No private VGM parsing,
conversion, compilation or playback was performed.

Code in mucom_fm.project_state computes (left_enabled or0)/(right_enabled or0).
This converts unobserved native values None to0 and conflates unknown source
pan with an observed hardware output mute. The existing three normalized State
CSVs contain no keyed rows with known left0/right0; they contain keyed rows
with both values unwritten. Thus the supplied rejection is a misleading
unknown-pan diagnosis, not evidence of an observed pan0 feature in these runs.

Primary reference: ymfm opn_registers_base<true>::reset sets B4..B6 on both
ports toC0 (both outputs enabled) before subsequent observed writes:
https://github.com/aaronsgiles/ymfm/blob/main/src/ymfm_opn.cpp
This is a reference playback reset profile, not a newly observed VGM write.
An eventual correction must retain source None and explicitly record an
output-only reset interpretation; do not globally mutate native Segments or
replace genuine observed0/0 with stereo. Verification must distinguish
observed source bits and recorded output assumptions.

Self-authored minimal MUC probes compare legacy and pinned multiport profiles.
Both compilers reject p0. Legacy also rejects numeric y180 and multiport y.
Pinned NET6 accepts y180,0 and y0,0,180,0; compiled native replay shows B4
C0->00->C0 with no additional key-on during the held note. Changing general
compiler choice cannot alone fix the current conversion, since the guard
raises before compilation. Explicit numeric or multiport B4 projection can
handle genuine mute with the supported profile; preserve AMS/PMS bits and
channel/port addressing, restore mute after H side effects if implemented.

Artifacts: outputs/diagnostics/fm_pan_zero_probe/summary.json and authored
MUC/DAT/MUB/VGM files. No production source/README changes, staging, commit or
push. Suggested priority: correct unknown-vs-observed pan diagnosis first,
then separately support true FM mute with the verified target dialect.
