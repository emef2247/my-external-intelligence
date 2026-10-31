# export_mdx PCM recipe defaults: --force alone should convert

Date: 2026-11-01
Status: adopted (branch claude/c140-mixdown, 92c9083)

## Context

PCM-bearing sources each needed their own extra flags at export time
(C140: --c140-projection mix plus a gain; PSG+SCC: --psg-output pcm
--psg-gain 2 --pcm-policy best-effort). The user asked for the export
layer to carry one standard recipe so that --force alone converts
them, in the same spirit as the export-layer buffer/policy defaults.

## What was decided

- C140 projection defaults to mix; the mix gain defaults to -12 dB
  (calibrated against FM on BURNFC03; the user auditioned it as the
  correct balance). Both remain overridable.
- PSG output is resolved per file at export: PCM with gain 2.0 only
  when PSG and SCC both sound; otherwise OPM. Rationale (user): PSG
  alone renders well on OPM; PCM is the fallback for the PSG+SCC case
  because SCC claims the OPM resources.
- --help lists only input / --outdir / --no-song-loop / --force; all
  other options stay accepted but unlisted (argparse.SUPPRESS).

## What to do next time

- Do not make --psg-output pcm a blanket default. vgm2mml accepts
  pcm only on the psg-scc-to-opm route and rejects it for OPM/C140
  inputs, so a global default breaks those conversions. Resolve per
  file via conversion_config.inspect_source + select_mdx_route,
  using effective chips (used_chips minus compatibility
  initialization keys) and requiring both psg and scc.
- The recipe lives in the export layer (CLI defaults and per-file
  resolution); run_batch and the canonical converter keep their own
  defaults (bank / 0 dB / opm).

## Verification

- BURNFC03 with --force alone produced a PDX byte-identical to the
  earlier explicit-flag build (--c140-projection mix
  --c140-mix-gain-db -12) that the user approved by listening.
- Full test suite: failure set identical before and after the change
  (28 pre-existing environment failures in the Muse VM; no new
  failures).
- SALAMD (PSG+SCC) end-to-end in the Muse VM stays blocked by the
  missing YM2151 core for automatic SCC voice fitting (lhasa is not
  installable there); route entry and PCM staging were confirmed,
  and emu2149 was set up via scripts/setup_psg_pcm.sh.
