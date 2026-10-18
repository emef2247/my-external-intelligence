# TBOUR finite PCM listening triage (2026-10-13)

User --no-song-loop export: TBOUR01/03 now succeed;02/04/05/06 still time out;
07/08 succeed. User hears FM but no PCM in TBOUR01. Screenshot MADRV with FM
activity; static screenshot does not certify PCM silence or resident state.
TBLADE finite export still running: do not touch/rerun its outputs/processes.

Read-only TBOUR01 static analysis, no private reconversion or compiler run:
- Native stereo1328926frames,359gates,0missingROMreads.
- Wave min-15737/max16142, native mono RMS2189.94 (-23.5dBFS).
-6final ADPCM samples, sizes62496x5+19584bytes; PDX332832bytes.
- First sample independent decode min-12032/max10880,RMS2116.73.
  It is not silent. Payload equals target encoded sample bytes.
- MDX10004bytes, names TBOUR01.pdx,16tracks, initialE8 onA, P has22NOTE,
  16KeyOffDisable,30controls,1rest,finiteEnd. Bank0, F4, pan3,gate8,
  maxdirectvolumeFB80. Short initial3ticks rest precedes first PCM note.
- Copied H:/_env/D/VGM2MML/opm_segapcm/no_loop/TBOUR01.mdx and.pdx exist;
  both SHA256 equal generated files. Copy omission/staleartifact ruled out
  for these files; this does not certify what emulated player currently loaded.

No cause established for inaudible PCM. Data contains nonzero audio and valid
static target intent; runtime PCM output versus relative loudness remains
unresolved. Do not blame missing PCM8 or user setup: prior notes confirm six
extended-mode cases sounded and ended in MADRV environment, bounded evidence.
Do not claim PDX byte equality means native playback correctness.

Asked user to try existing authored SEGAONE MDX+PDX at
outputs/segapcm_pdx_check/tracks/SEGAONE (short low PCM tone concurrent with FM).
This isolates private sound/masking from general target playback, without
private fixture reruns. Await user's result. No algorithm or default changed.

Decoded only existing PDX slot0 to ignored outputs/segapcm_pcm_diagnostic/TBOUR01_slot0_decoded.wav for user comparison:124992samples/7.999488seconds at15625Hz, signed16bit mono, no gain. Diagnostic decoded sample is not a native MDX runtime capture. No private VGM conversion or compiler invocation.

