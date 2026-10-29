# Explicit PDX whole-file limit — 2026-10-30

User requested export_mdx argument control while testing MXDRV's PCM-buffer
setting. Added --pdx-limit-kib N to shared canonical/export parsers and batch
API. It sets whole encoded-file bytes, tables and padding included;1024bytes
per KiB, integer positive input, file cap constrained to u32 representation.
Default with no resident setting remains315122bytes. Explicit file values can
exceed the former1200000byte ceiling. An explicitly configured resident buffer
still constrains the effective file size after loader overhead and margin.
Without the file override, configured residents keep the1200000byte ceiling.

The existing --pcm-buffer-kib is different: it describes the actual player
allocation and does not configure MXDRV itself. Keep CLI explanations clear.
Immutable capacity policy is propagated into bank selection and final actual
file checking; metadata records requested bytes and effective budget. Statistics
reconstruct the explicit request from current metadata instead of reverting to
the old ceiling. No source evidence, sample rate, slots, or guest settings change.

54 existing/updated checks pass: policy12, capacity stats9, export33. No private
music conversion or playback in this task. Broad regression remains with user.
Preserve all earlier uncommitted MSM6295/capacity work. No commit/push requested.


## Superseding user decision: use the buffer size as the limit

The user explicitly asked to derive the file limit from the buffer size alone.
Removed the just-added --pdx-limit-kib/independent file-limit field from CLI,
API and metadata reconstruction. --pcm-buffer-kib is the sole capacity input:
PDX budget=buffer bytes-loader overhead-margin. Removed the fixed1200000byte
ceiling, so larger buffers actually permit larger PDX files. Omitted buffer
retains315122B. --pcm-buffer-kib1024 --pcm-loader mxctrl permits1044210B;
2048KiB permits2092786B. The player allocation is configured separately by the
user, never changed by the converter. Old snapshot notes above describe the
withdrawn intermediate design and must not be implemented again implicitly.

Final unified setting verification: policy12 + stats9 + export33 =54passed.
No private conversion/listening or commit/push. Current docs and handoff updated.
