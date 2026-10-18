# SegaPCM native evidence and OPM-only diagnostic checkpoint (2026-10-13)

Context: user wants to test MDX FM compatibility before investing in SegaPCM
transcoding/PDX. This checkpoint is native source analysis plus explicit
--fm-only output; it is not completed SegaPCM playback reconstruction.

C0 LE16 operands and type80 sparse ROM blocks now share the existing VGM
sample/event/loop traversal. Immutable write-observed states/control Segments
retain unknown values, programmed vs runtime addresses, repeated requests,
ROM generations and source provenance. EOF is not a synthetic disable.
Runtime advancement/natural completion remain unmodeled. Malformed ROM bodies
are retained opaque; truncated command framing remains an error.

Default mixed conversion returns source_analyzed only. --fm-only explicitly
omits SegaPCM target audio, saves native evidence, and reuses existing OPM
pitch/clock/loop/structure and MXC export. JSON/TXT report the omission.
The option must not bypass unknown source/stream validation, and SegaPCM
control slices must not constrain FM normalization. best-effort is not consent
to silently discard the chip.

Five authored source/CLI/export checks and26 existing exporter checks passed.
Authored mixed input and equivalent OPM-only input produced identical FM MML
including declared-loop structure. One authored MXC/run68 export produced
MML/MDX/TXT without PDX; native CSV/ROM/report evidence was inspected.
No private source conversion or native playback was run. No Git mutations.

Next: stop for user regression/listening. SegaPCM->PDX must wait for those
results/authorization. Consider alternate targets only if the existing MDX
FM compatibility findings warrant it; no Z MUSIC implementation was started.

## Listening staging interruption recovery

An interrupted export left a byte-identical staged input without its manifest
entry. Preflight correctly prevented overwrite but also prevented harmless
reuse. Adopt only unregistered staging whose digest equals the current source,
without copyfile; recheck before recording ownership. A differing unowned file
or any registered modified staging remains rejected, even if the current source
has also changed. Publication ownership checks remain unchanged.
Ten focused authored listening-export tests passed, including interrupted
identical staging recovery without overwrite and differing/modified rejection.
Private data was compared read-only for equality; no private conversion ran.
