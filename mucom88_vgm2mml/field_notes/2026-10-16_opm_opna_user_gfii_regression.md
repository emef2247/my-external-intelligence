# User OPM/SegaPCM GFII regression (2026-10-16)

User reports intended real-VGM conversion with --pcm-policy best-effort --force
--opm-channel-policy omit on GFII directory.8/10 success. GFII04/06 conversion
timeout after180s at external_pcm_projection (not OPM projection or compiler).
No cause investigation or rerun requested/performed for these timeouts; default
remains180. GFII01/02/10 zero omissions;03 one PCM payload omission270000/262144.
05:17 PCM payload omissions plus OPM G/H (8/6 ch);07:16 PCM payload omissions,
G/H and one DT2 registerC0 mask192 omission;08:10 PCM payload omissions plusH
(7/6 ch);09:G/H channel omissions. Counts are projection decisions, not source
write/audio-equivalence counts. User confirms intended conversion; do not claim
all10 passed or independent playback validation.

User prepares commit/PR on codex/opm_to_opna, requests concise English message:
Add OPM-to-OPNA FM projection with explicit loss diagnostics
Next authorized work is x68k shared PSG/SSG note interpretation and scoped noise.
Public mucom files left untouched while user's commit is pending. No reruns/tests,
Git stage/commit/push or branch changes for this evidence record.
