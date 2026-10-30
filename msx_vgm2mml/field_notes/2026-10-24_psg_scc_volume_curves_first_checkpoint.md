# PSG/SCC complete-volume-curve first implementation checkpoint

2026-10-24. User authorized six gra2_001 with_sync_mark VGM/MML cases and later
the parent gra2_001.vgm/MML. No unrelated local_only conversion is authorized.
The reference score remains an independent oracle, never a production input.

Implemented opt-in --software-volume-curves, derived immutable PerformanceIR,
ordered native zero-time-operation barriers and separate candidate/note/decision
CSV evidence. Complete fixed-pitch single-rise-then-decline curves feed loop
planning before the shared envelope bank. No lossy smoothing yet. Defaults stay
unchanged. Worktree checkpoint only, no commit/push.

Six excerpts: PSG joins24; SCC joins198; units6398 ->6176. MML49918 ->49510B.
Padded MGS32512 ->33664B is NOT the user's primary metric. User clarified that
MGSC per-channel buffer allocation/usage and channel-specific bloat are the
priority. Use `mgsc input output -T` for actual allocated/used/free bytes; do not
report proportional #alloc shares or MML text estimates as compiled usage.

Full song: PSG4002 ->3978 units; SCC2634 ->2433. MML65773 ->65841B. Both normal
16k pool compiles fail Track buffer full (lines239/243). Reference compiles and
its MGS re-export is byte-identical to input VGM. Diagnostic-only enlarged pool
copies are being measured to obtain real per-channel usage, not as a production
fix. Six cuts have different boundaries/global banks, so their sums cannot stand
in for the complete-song transformation. Snapshot reports are under outputs/
diagnostics/psg_scc_curves_impl_20261024/{verification.json,full_song/}.

All six matching reference MGS files re-export byte-identical source VGM. Intended
renderer timelines and source maps are unchanged between baseline/projection.
Real replay04/05 apparent pitch/attack errors on a fixed grid are short write-
scheduling jitter: collapsed stable states match, SCC deltas<=70samples. Case01
has real volume timing drift, up to7frames; do not confuse this with CPU jitter.

Authored PSG/SCC probes prove MGSDRV advances the SW envelope an extra step on
continued tied notes. At tempo225 a max255-step note spans85frames. Seven tie
boundaries account for7frames advance of the long joined SCC curve. Tempo75
probes confirm the same behavior with explicit identical split schedules. Note
end timing is unaffected. Padding each hold for tie boundaries in [start,end)
works in36 diagnostic cases;16 reusable subcases are in
 tests/script/test_mgs_tied_envelope_playback.py. This correction is NOT yet
integrated: use the final emitted note split schedule, and make envelope sharing/
prefix reuse aware of the compensated curve. Normalization can change that
schedule. Do not apply a blanket hold extension or assert exact backend replay
before integrated verification.

Next: finish per-channel/original-envelope audit, implement final-clock tie-aware
encoding/sharing, replay six cases and full diagnostic copy, then reassess
compression. Avoid promoting single-attack inference to recovered source Key-On.
