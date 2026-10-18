# Direct PSG/SSG and MSM6258/DELTA force (2026-10-16)

User committed the sync checkpoint, then authorized PSG -> OPNA SSG and
MSM6258 -> DELTA plus MUC --force first. "MSM6358" was interpreted as existing
MSM6258. OPM/SCC target conversion is still later work. No Git mutations.

## Source and target architecture

New psg_mucom typedPSG intervals retain actual0xA0 register order, clock/type/
flags, reset assumptions and source IDs. Do not reinterpret them as OPNA native
State/trace. Common SSG target/render/loop machinery consumes the real command
and an explicit effective clock. No PSG -> OPM -> OPNA roundtrip. Master/4 ratio
belongs to target projection; native AY clocks remain intact. YM2149 Pin26 half
clock is modeled per VGM header. Analog amplitude/EG-phase identity unverified.

The G lane carries original shared-envelope controls in same-time source order.
This is a target global-control workaround, not synthesized native rhythm.
Default shared-envelope conflict remains unsupported; force splits/mutes the
conflicting set only over overlap windows, preserving supported intervals and
source members. Noise-only output ignores stale inactive tone registers; tone-
only output ignores inactive noise registers. Period failures and prescale
failures are explicit interval omissions. Source values are never quantized.

Force source exclusions are explicit for known unsupported OPM/SCC/OPLL engines;
these are not converted FM/waveform parts. Mixed nativeSSG has DEF priority and
externalPSG is omitted under force. Malformed input/unrecognized streams stay
errors. NativeOPNAFM/PCM guard coverage was not globally expanded by this task.

## MSM6258 and capacity

Reuse existing native IR -> decode -> ADPCM-B -> K target adapter. Force32slot,
sample-length and256KiB payload checks run after encoding. Oversized new chunks
become timed gaps; retained digest reuse remains possible after full bank.
All-PCM-omitted paths produce no empty bank/#pcm and preserve song end/loop.
Default best-effort remains a separate explicit policy. Known unsupported MSM
HW/heldpan may omit entire part/playback; unresolved supply cadence/reset/sample/
clock errors remain fatal, even when accompanied by an allowlisted rate-change.

mucom_force registry preserves original detail objects, IDs/times and exact
actual/limit; CLI and batch/TXT consume the same records. Compiler status is
never promoted to success. Default timeout and sampling quality unchanged.

## Evidence and corrections

23 distinct authored checks passed:11PCM,3force,3CLI,5PSG,1nativeSSG provenance.
Initial PSG test caught a hardcodedssg: namespace in slice membership, fixed
without changing source evidence. Architecture review identified O(N²) tail-list
copying in conflict detection; replaced with active sweep and indexed conflict
queries. Review also caught inactive tone range-check on noise-only PSG; fixed
and scoped new authored check passed. Native PCM codereset/cadence guards reviewed.

Persistent authored canonical PSG+MSM default-normalized MUC has D/K tracks,
1PCM binding,0force omissions; patched4byte capacity case keepsPSG and removes
PCM/#pcm with1omission. Native source/control/unit/mapping/force artifacts inspected
under outputs/diagnostics/psg_msm_mucom_20261016. These are not native compiler,
real playback or hardware waveform checks. No private fixture/large regression.

README is concise; docs/psg_to_ssg.md anddocs/mucom_force.md carry details.
Current handoff replaced obsolete paused-state history with current scope.
User regression/listening is next. No implicit SCC/OPM or compiler fallback.
