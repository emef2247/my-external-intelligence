# Shared export package layout verification (2026-10-24)

User confirmed all three repositories are in scope. Each original input parent
becomes an output package, preserving its INDIR-relative parent. Final editable
scores, compiled binaries and required banks are directly in tracks/, without a
per-song directory. Diagnostics remain per song; staged original source copies,
listening_manifest.json and package-local results stay separate. Omitted outdir
uses adjacent packages. No music interpretation or threshold changes were made.

Validation used public/authored inputs only; no local_only data was converted.
Related focused checks: MSX 54, X68000 51, MUCOM 40 passed. Architect review found
and then rechecked fixes for metadata-file versus package-directory collisions,
case-insensitive package aliases and MUCOM ownership-commit rollback.

Independent CLI checks cloned public fixtures into a root input and two nested
game directories, with repeated SONG01 stems across packages and SONG01/SONG02
within a package. Both MGS entry points, export_mdx and export_mucom compiled
all four inputs. Assertions checked flat products, original bytes/hashes,
manifest references, package results and a one-file rerun retaining the other
song and its result row. Outputs are ignored local artifacts:
- msx_vgm2mml/outputs/package_contract_0esd6vrz (export_mgs)
- msx_vgm2mml/outputs/package_contract_ka8owzvi (batch_vgm_to_mgs)
- x68k_vgm2mml/outputs/package_contract_wt_fwt4p
- mucom88_vgm2mml/outputs/package_contract_47d40nkb

Public SEGAONE additionally compiled with PDX at
x68k_vgm2mml/outputs/package_layout_pcm_20261024. tracks contains SEGAONE.mml,
SEGAONE.mdx and SEGAONE.pdx. The compiled MDX reference is SEGAONE.pdx;
the PDX is 8960 bytes and its publication hash matches the manifest.
MUCOM worker also verified real native MUCOMSX MUB compilation under
outputs/package_layout_20261024/mucomsx. These are packaging/compilation checks,
not a new claim of listening equivalence or runtime PCM buffer fit.

Failures retain previous complete listening products and record latest attempts.
Owned output hashes are verified before replacement. Legacy layouts are not
automatically deleted or migrated. Companion basename conflicts are explicit
errors rather than binary-reference patching. Stable contracts are documented
in each repository's docs/output_layout.md. No staging, commit or push.
