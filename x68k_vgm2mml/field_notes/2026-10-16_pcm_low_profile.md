# Optional lower-size PCM target profile (2026-10-16)

User clarified both repos/sources: MUCOM MSM6258/SegaPCM and x68k Delta-T,
MSM6258,SegaPCM need a smaller-output choice for frequent native PCM buffer
errors. Implemented --pcm-profile standard|low; no commits/staging/pushing.
Preserve State/Segments/PCM IR, defaults and all unresolved source/target guards.
Existing source principles and earlier Sega buffer/listening notes were read.

Shared immutable pcm_profiles.py defines standard15625Hz/F4/div512/8sec and
low7812.5Hz/F2/div1024/16sec. x68k waveform bridge uses per-analysis profile for
chunking, reencode/cache, shared MDX-clock alignment, positive even counts and
reports. Native full-source waveform still44.1k; no claim total analysis memory
or conversion timeout is halved. Source-specific clocks/decoder stages intact.

Independent Delta caps nearest target F toF0..F2; continuous Delta and Sega use
common lower-rate waveform bridge. Native MSM's optional target helper retains
original source dump, controls/IDs/reset/cadence/issues, and only reencodes
eligible known decoder samples under explicit nominal decoder model. Source
encoded bytes retained separately; low target dumps .pcm_low.pcm_*. Byte-identical
new samples deduplicate. Already within cap preserves bytes and reports0
reencoded playbacks/no false loss. Nonpositive/unknown/unsupported evidence
passes to existing reject paths rather than being resolved by quality choice.
Per-playback loss/mapping rates, bytes, duration error, actual F and original
source times remain inspectable. Profiles are target-only; best-effort required
when known quality loss occurs. Same96slots/65535byte scope/force selection.

x68k tests5 new (nativeMSM halfpayload/source unchanged, already-low/noop+source
12-bit blocker, Sega chunk/alignment rate, independent/live Delta rate, batch
forwarding) +3existing Sega +1existing live Delta allpass. Authored standard
waveform compared with HEAD by old-module execution: exact encoded bytes and
playback bounds/rate match. Native MSM default helper returns original object.
Sega20sec total packaged bytes half, chunks3->2 (test encoder stub; not audio).

Persistent authored outputs outputs/diagnostics/pcm_profiles_20261016:
nativeMSM200->100 bytes, same source gate, typed F2; separate native/derived dumps
and profile JSON. Actual canonical MSM CLI generated MML/MDX/PDX under MSM_CLI.
Public self-authored SEGAONE canonical CLI standard/low generated helpers'
MML/MDX/PDX and inspected mapping/bridge/timing: payload8190->4095, rate15625->
7812.5, F4->F2,1sampleeach, source hash unchanged. No private conversions,
external mxc/mmlx compiler run or listening; helper binary used only authored.

MUCOM worker parallel scoped implementation saved sameoption/config, dynamic
Delta-N/rate for counts/chunks/MUC pitch/semantic keys/reports;14focused tests
pass, standard bank/items matchHEAD. Authored SEGAONE8196->4096 bytes with
actual Delta-N18462->9231 (rate15625.390625->7812.6953125). Native OPNA ADPCM
not in requested MUCOM scope, reports inapplicable and unchanged. Its own note
has exact verification. No x68k target constraints imported as MUCOM rules.

Architect initial review unavailable due quota, resumed final read-only review
succeeded. No blocker; flagged no-op native loss and largest-chunk wording.
Both handled: only actually changed rate playbacks record requantization loss;
low16sec maximumchunkstillabout62500bytes, same as standard8sec. Totalpayload/
slotcount can shrink, singlechunk buffer requirement need not. Source gaps,
loops, exact byte dedup, padding/alignment and force omissions affect ratio.
No guaranteed runtime buffer fit. Existing linear resampling no AA filter:
high-frequency loss/aliasing and ADPCM errors remain explicit. No new filter
or memory-budget truncation. Default timeout unchanged180.

User regression/listening next. Docs READMEshortlink->docs/pcm_profiles.md;
related source/PDX docs link profile scope. Handoffs updated. Private note
changes preserved; public commits do not include these external notes.
