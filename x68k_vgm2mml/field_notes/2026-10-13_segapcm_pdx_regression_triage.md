# Full SegaPCM regression failure triage (2026-10-13)

Read existing user results/logs/artifacts only; no private reconversion or
compilation. Full PCM best-effort mmlx exports: TBLADE5/12, TBOUR2/8.

Four conversion_failed results reach typed PCM MDX construction after waveform,
bridge final shared-clock alignment, target samples and PDX packing complete:
- TBL02, TBOUR01, TBOUR03: FM song loop does not target an outer command boundary.
  helper fm_track_timing records byte->tick boundaries only outside finite
  repeats, then resolves backward EndOfTrackLoop against those boundaries.
- TBL10: FM/PCM song loop start and end must match on every active track.
  helper requires each active FM track to equal P plan L tick and end_tick.
All four generated FM MML have one L on every A-H track. Missing VGM loop/L is
not the cause. Exact compiler offset/tick discrepancy is not yet isolated;
do not claim malformed source, true unrepresentable input or capacity limit.
Their selected ticks are15872,9216,7424,6656us respectively, not256us.

Nine conversion_timeout results hit outer conversion wall budget180seconds:
- TBL01/04/11/12 and TBOUR04: native waveform/gates/runtime manifest complete,
  bridge manifest absent. Stop lies in initial target bridge before its dump.
  ADPCM/resampling cost is inferred from code; no runtime profile was taken.
- TBL09: initial bridge manifest and final MDX normalization JSON present,
  bridge aligned_sample_multiplier stillNone. Final clock re-encoding/dump
  is unfinished. Current implementation may encode twice if samplecounts differ.
- TBOUR02/05/06: source collector manifest exists but no runtime waveform or
  playback manifest, so runtime rendering or preceding source-pass completion
  has not reached its checkpoint. Exact active hot function unknown.
Expanded source seconds: TBL01=619.70,04=403.47,09=321.12,11=518.47,12=473.56;
TBOUR02=386.80,04=376.40,05=530.53,06=469.34. Durations include reader loop replay,
not necessarily original one-pass song duration.

Encoder encode_msm6258 is Python, evaluates16 candidate nibbles per15625Hz
sample, plus resampling each output sample. Runtime renderer is Python over
native frames and activevoices; longer full-PCM paths add substantial work.
These timeouts are not driver timer periods or a compiler capacity diagnostic.
Raising outertimeout may finish these stages but cannot fix four loop-validation
failures and does not guarantee no later target limit. --no-song-loop avoids
song-loop matching by finite output; untested here. No defaulttimeout change.

Incidental sparseROM substitutions: TBL04=47,TBL09=4,TBL10=13,TBL11=38,TBL12=6
active reads. These are best-effort diagnostics, not blocking cause.

Next useful implementation: isolate typed FM loop offset/timing validation with
small authored reproduction; reduce duplicate encoding and Python per-sample
cost, preserving exact source and target semantics. User asked diagnosis first;
no implementation fix performed in this triage.
