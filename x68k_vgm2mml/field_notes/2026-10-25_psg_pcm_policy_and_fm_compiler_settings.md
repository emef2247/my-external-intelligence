# PSG PCM policy and typed FM compiler settings (2026-10-25)

The user's existing SALAMD01 best-effort diagnostics identify native MDX track
2 offset overflow (0xfffe), not PDX capacity: encoded PDX was694338bytes within
the1200000-byte policy. Readable MML was54548bytes; actual .pcm/opm.mml was
476015bytes with48352direct-register writes. No private conversion was rerun.

Cause: opm_conversion's readable structured build passed short_state_samples
from the selected fm_state_ms, but its second FM-only build for typed PCM
assembly omitted the option and used zero. Both now consume structure_options;
normalization OFF gives zero in both. Before-normalization dumps remain baseline.
This fixes the compiler path's missing setting without broadening approximation.

Auto now permits known transcoding losses for the explicit PSG PCM source origin
projected_msm6258_from_psg, reason explicit_psg_pcm_projection. Explicit strict
still blocks, explicit policies override auto, native direct remains strict,
and observed DAC supplies still select best-effort. Other projected origins are
not generalized. Source interpretation failures remain blocking.

Authored regression captures the FM structure passed to write_mdx and compares
all FM track lines/short-state omissions to the published MML for24ms,8ms and
normalization OFF. This would fail before the missing-option correction. Tiny
PSG PCM projection validates default auto and explicit strict. Existing native
policy coverage passed. Total focused tests:19. Full private regression and
playback remain the user's responsibility. README and staged earlier changes
were preserved. No stage/commit/push.

Final validation: public psg_scc_001 exported MDX/PDX successfully with --psg-output pcm --scc-model additive --no-song-loop and no policy override; auto selected best-effort. Focused review found no critical issue. SALAMD01 itself has not been reconverted.

## User regression and listening confirmation

The user reran SALAMD01 with --psg-output pcm --pcm-policy best-effort after
the FM compiler-setting correction and reported success and audible playback.
Reported output: 12 samples / 12 playbacks on P, zero omitted playbacks,
693570 bytes of sample payload, EX-PDX total 694338 bytes within 1200000.
This is user-reported conversion and listening evidence for this song. The
runtime-buffer-fit metric remains unmeasured; do not generalize to all files
or call it an instrumented hardware equivalence/buffer-capacity measurement.
The successful rerun explicitly selected best-effort; the new auto default was
separately covered by the authored tests and public conversion.
