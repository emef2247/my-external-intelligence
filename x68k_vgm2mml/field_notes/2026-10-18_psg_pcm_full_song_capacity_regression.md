# Full-song PSG PCM regression: native track capacity (2026-10-18)

User finished native regression. Command selected --psg-output pcm, best-effort,
mmlx, no-vgm. A separate shell line ran --force and produced command-not-found;
all five successful conversion JSONs show force_requested=False. Force would
not waive MDX offsets or MADRV runtime buffers, nor repair helper loop structure.
No conversions/tests or playback rerun for this audit; only existing files read.

User reports native playback only GRA2_02/GRA2_17 succeeds; GRA2_07/10/11 show
MADRV track-buffer shortage. Exact installed MADRV buffer configuration is unknown,
so no universal32K/per-track capacity assertion is made from this screenshot.

Existing results.csv categories:
- 7 pcm_projection_blocked are actually native MDX track-start offset >0xfffe:
  GRA2_01/03/04/06/09/13/16. This status arises from typed PCM assembly and does
  not mean PDX/sample capacity caused these particular failures.
- 5 conversion_failed are FM song loop does not target an outer command boundary:
  GRA2_05/08/12/14/15. The helper boundary map only records outer commands;
  this is a separate FM-loop/typed-assembly issue, not simply data size.
- 5 generated MDX/PDX pairs:02/07/10/11/17; native success only02/17 peruser.

Read original16track MDX offsets, bounded by nexttrack/tone/fileend. Measurements:

name | total MDX bytes | FM A-H bytes | PCM P command bytes
--- | ---: | ---: | ---:
GRA2_02 | 7107 | 6200 | 17
GRA2_07 | 62255 | 61555 | 129
GRA2_10 | 64521 | 63449 | 171
GRA2_11 | 52391 | 51819 | 135
GRA2_17 | 5210 | 4730 | 15

All target row source_chip values in these outputs are SCC; projection setting
psg_output=pcm. FM A-H constitutes >98% of 07/10/11 MDX; PCM audio bytes are
separatePDX and must not be confused with PCM command-track bytes above.
07/10/11 PDX file sizes301266/534868/401358; native ADPCM buffer may independently
become a later issue, not established as the displayed track-buffer cause.

07/10/11 volume-control byte counts14250/13138/12244 (FA/F9 onebyte, FBtwobytes),
KeyOffDisable7856/6904/6329bytes, raw registerFE15408/16299/12051bytes. Needed
controls are not proven redundant merely by counting; preserve trajectories.
Existing structure metrics emitted loop commands65/9/8 respectively; compacted
MML lengths134195/143047/117851 versus plain241360/230321/192403. Repeat folding
is active; those are character metrics, not proof of binary-capacity adequacy.

Primary localcode findings:
- msx py/mml_envelopes.py uses reusable @e curves then one duration per note;
  MDX py/opm_mdx_music.py renders volume-run pieces via @v and timed held notes.
  Same source-note/loop interpretation does not imply same target byte cost.
- docs/mgs_compatibility.md already states automatic macros optimize source
  characters and need not reduce compiled track memory.
- local mmlx0.2.0 grammar/compiler has no MML macro/phrase-call facility; supported
  soundlog0.15.0 MDX command enum/dispatch has finite repeats/song jump, not a
  generic runtime phrase-call+return. Do not promise lexical macroization saves
  standard MDX bytes. Do not assert MGSDRV macros always expand without evidence.
- helper scripts/mdx_fixture_generator/src/main.rs:293 checks loop destination
  onlyagainstoutercommand boundary map; thisguardcauses above5errors.

Recommendation: separate fixes (loop assembly/boundary handling) from binary
size optimization. Prefer byte-aware control/state dedup and volume-trajectory
representation/adjacentrepeat optimization; preserve sound/Key/loop semantics.
Generic macro-only source compression is not a verified MDX size solution.
No implementation change or test introduced by this diagnosis.

Prior unanswered balance question can already use --psg-gain0.5 for half target
PCM amplitude, or --scc-gain0.25 versus default0.125 (autobank refit/gain changes).
Recommend attenuating PCM first to preserve chosen SCC fit and OPM headroom.

## User rerun after loop fix

Reviewed existing psg-pcm_mtx and psg-pcm_mmlx results.csv, with force and
psg-gain0.8. Both17entries:10success,7MDXoffset0xfffe blocks (01/03/04/06/09/13/16).
Allfive prior loop-assembly failures now export. User reports native listening
works only02/17; othergenerated songs fail MADRV trackbuffer. No sourceHW/PCM
capacity omission reported. No private tests/reconversion run. User deprioritized
MUCOMSCC and forked scc_vgm_enhancer to add SCC-derived OPM/OPNA to preserved
original SCC/PSG VGM, avoiding MML/MDX packaging for listening.
