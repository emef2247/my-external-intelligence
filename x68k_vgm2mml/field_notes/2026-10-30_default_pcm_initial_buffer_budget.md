# Default PDX cap based on initial PCM allocation — 2026-10-30

User explicitly requested lowering the former1200KB default to the previously
examined initial PCM buffer size for their own playback regression. Multi-bank
storage was proposed as a possible later solution; no multi-bank implementation
was requested for this small change.

Reference: examined MXDRV2.06+17 Rel.X5-S initial allocation312KiB. This is not
proof of the installed MADRV/MXDRV allocation. Use the existing conservative
MXCTRL270-byte wrapper and4096-byte headroom:319488-270-4096=315122bytes.
The default table-inclusive encoded-PDX budget is now315122B (26.26% of1.2M).

Keep a separate PDX_OPERATIONAL_BYTE_LIMIT=1200000. Explicit --pcm-buffer-kib
settings still resolve min(operational ceiling,buffer-loader overhead-margin),
allowing users who configured larger residents to opt up. Omitting the buffer
uses the new conservative file cap; actual resident remains unconfigured and
runtime playback unmeasured. No emulator or guest settings were changed.

The existing immutable capacity policy propagates this default to bank96
representative selection, force/binding and final actual packed-size validation.
No new silent omissions or waveform truncation. Source evidence remains intact.
PSG continuous mix retains its prior behavior and may fail a tighter capacity
check instead of becoming a reusable bank automatically.

Multiple96-slot banks solve slot addressing only if supported; loading all of
them still consumes their combined resident memory. Future work must separate
bank-local size from total loaded bytes and establish actual driver bank-switch
or load behavior, active voice lifetime and ADPCM decoder continuity. Do not
claim bank splitting alone fixes the buffer issue.

Minimal verification: capacity policy11, budget selection8, capacity stats9
checks pass (28 total); git diff --check clean. No local-only conversions or
listening. User performs regression. Preserve unrelated uncommitted MSM6295
and earlier capacity-policy work. No commit/push requested in this turn.
