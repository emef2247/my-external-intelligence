# 2026-10-28: export boolean switch cleanup

User requested one visible switch per default-enabled export feature and asked
to stop further filesystem investigation due remaining quota. export_mgs help
now exposes --legacy-macros [BOOL] and --no-software-volume-curves [BOOL].
Bare switches activate the named behavior; optional values accept yes/y/1/true
or no/n/0/false case-insensitively. Internal booleans match their meaning:
is_legacy_macros=False and is_software_volume_curves=True by default.
Older positive switches remain hidden compatibility aliases. Preserve a
positional input after a bare flag; optional bool parsing must not swallow it.
19 focused export/publication tests passed. Original batch defaults and README
remain unchanged. No commit/push requested.

SALAMD Track buffer full is separate from the later FileNotFoundError during
product replacement. The tracks path was a normal755 directory without an
explicit Windows ACL deny; no root cause is established. Before scope reduction,
verified-copy fallback/rollback and per-file publication_failed diagnostics were
added and tested. Do not claim this proves why the original ENOENT occurred.
User will compare another output location/free space; do not rerun private data.
