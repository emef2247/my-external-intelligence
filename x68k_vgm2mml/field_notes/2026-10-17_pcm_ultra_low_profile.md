# PCM low user regression and ultra_low target profile (2026-10-17)

## User-supplied observations, not agent reruns

User reports low still often displays native ADPCM buffer insufficiency during
playback, despite generation success. They request ultra_low (=1/4 standard) and
batch timeout default600 seconds. No precise per-song player-memory limits or
loaded bank sizes supplied; conversion success is not playback/load validation.

Flags: --no-vgm --pcm-policy best-effort --pcm-profile low --compiler mmlx
--no-song-loop --force. TBOUR default180:4/8 generated,01/03/07/08 success;
02/04/05/06 conversion_timeout. With600:6/8,01/02/03/04/07/08 success;
05/06 pcm_projection_blocked. Pasted statuses alone do not identify those
projection-block causes. No new investigation or private reruns performed.
Retained sample counts01..08:3,13,1,12,17,15,6,4; no capacity omissions reported.

TBLADE default180:11/12;11 conversion_timeout. With600:12/12 generated.
Retained counts01..12:20,4,1,15,5,14,1,92,21,10,96,19. TBL11 force report omits
12 new samples/playbacks IDs96..107 at attemptedslot97/limit96 (slot_capacity).
Successful generated output can therefore be partial; not proof of audible PCM
or native buffer fit. User owns listening and real regression.

## Implemented target choice

Shared immutable pcm_profiles.py matches both projects exactly:
standard15625Hz/F4/8MHz/div512/8sec; low7812.5/F2/8MHz/div1024/16sec;
ultra_low3906.25/F0/4MHz/div1024/32sec. Waveform analysis/PcmPlayback/bridge must
all record F0's4MHz clock, not leave the previous8MHz constant. Counts, shared
clock alignment, caches and reports consume per-analysis profile. Native full
waveform/source IR untouched; target sample count nearest positive even value.

Native MSM and independent Delta cap ultra toF0; eligible native MSM reencodes
with same source gate IDs/bounds and separate .pcm_ultra_low.pcm_* dump. JSON,
mapping and loss names reflect selected profile. Already-F0 bytes remain exact
with no fake loss. Existing reset/cadence/unsupported source guards preserved.
Same96slot/65535byte verified target scope and force behavior; no arbitrary new
player-memory budget. Different-profile reprocessing requires original IR,
not a second resampling of an already-derived profile.

Inverse-rate32sec waveform chunks retain maxabout62500bytes, like standard8sec
andlow16sec; benefit is aggregate bank reduction and fewer slots, not smaller
worst-case single sample. Short gates, loops, dedup and omissions alter ratio.
Independent Delta/native MSM are separate from waveform chunking and can still
exceed per-sample scope. No runtime buffer-fit guarantee. Linear resampling has
no AA filter; high-frequency loss/aliasing and ADPCM loss explicit.

MUCOM MSM/Sega nominalquarter derives integerDeltaN4615, actual1499875/384Hz;
its count/chunk/MUC/semantic-key/report calculations use that actualrate. Native
OPNA ADPCM remains inapplicable. MUCOM32slots/256KiB and MDX limits distinct.
Both user-facing batch signature/parser defaults600 seconds perstage/input;
explicit timeout override preserved, internal helper budgets not blanketchanged.

## Focused evidence after files saved

x68k7 authored profile tests pass: existingstandard/low, ultra nativeMSM200->50
bytes/sourceimmutable/F0, already-F0 unchanged, derived40secmappedpayloadquarter,
32secchunks, finalalignmentwithinone selectedsample,4MHz/div1024 dump, independent/
liveDelta ultra, batch default600 and explicit17 parser/forwarding. Initial new
test dump used empty fake source header and failed source validation; corrected
authored test source header, did not relax production guard. Rerun7/7 passed.
MUCOM3 focused authored checks passed per worker; no private tests/compiler.

Public authored SEGAONE canonical command:
python vgm2mml.py tests/fixtures/public/opm_segapcm/SEGAONE.vgm
 --outdir outputs/diagnostics/pcm_ultra_low_20261017 --pcm-profile ultra_low
 --pcm-policy best-effort --no-song-loop --dump-passes
 --pcm-generator scripts/mdx_fixture_generator/target/release/mdx-fixture-generator
Generated MML+MDX+PDX. Inspected bridgeclock4M/div1024/F0/rate3906.25;
MML P @0 F0 p3 q8 @v127 n0,%63 r%27. Payload2048bytes confirmed in PDXtable,
PDXfilesize2816 (768header), MDX172bytes. Prior standard8190/low4095 payload,
rounding prevents exact4:1 on short source. Source passes retained. Native
load/playback/display untested. Architect focused review supported inverse-rate
chunking with explicit buffer qualification. No Git staging/commit/push.
