# Shared PSG/SSG source notes and scoped noise (2026-10-16)

User authorized roadmap2/3 after OPM->OPNA GFII regression. No public mucom
edits while its commit/PR is pending; all implementation in x68k working tree.
No Git mutations. Prior decision audibility-only was reviewed in
2026-10-08_psg_opm_musical_connection.md; latest request intentionally supersedes
it for PSG using MSX shared musical interpretation. SCC policy unchanged.

New source-sample adapter wraps existing mml_envelopes.extract_notes, same
mechanism as mucom's adapter. Decreasing/steady volume runs coalesce; rises,
pitch writes and settings/shape changes split inferred units. Native Segments
intact; zero-time source controls survive without manufacturing isolated gates.
Target uses note IDs plus actual channel mapping; no MGSDRV syntax/grid/bank
limits imported, no claim of identical MGSDRV structural compression.

Authored artifact inspection found legacy PSG final row [110250,110250] muted,
followed by raw wait until132300. Blanket zero-row suppression delayed inferred
Key-Off despite real LR mute. Adapter now carries final known state through
observed EOF only, reporting source_interval_extensions and separate interpreted
end while retaining native end. Final release110250 matches SSG. Architect
confirmed this is bounded known-state carry, not an invented release tail.

Noise scope: FM model, one positive-duration fixed-level noise-only source part
fixed whole song. OPM7/C2 AL7, otherTL127. Standalone permutes PSG5..7; OPNA
keeps FM reservations and original source-order capacity selection, then reserves
actual7 for eligible retained noise. reg0F exempt from ordinary channel remap.
Tone mode restores profile andNE0; finite end disablesNE; song loop retains it.
HW-EG, mode3, other noise owners, unavailable7, additive noise reject or explicit
force mute. No silent noise mixing/ch stealing. Zero-duration noise cannot
reserve ownership or block a real part; source row and approximation retained.
Candidate eligibility also uses bounded EOF state carry consistently.

Nominal AY shift clock/(16*max(1,period)), OPM4000000/(32*(32-N)); choose minimum
log-ratio error forN0..30. N31 exclusion is conservative policy, not hardware
assertion. Different LFSR/phase/spectrum/gain, no listening equality claim.
MAME AY and YMFM OPM divider/register sources consulted (links in focused docs).
Per-row source noise period/effective clock/source rate/selectedN/target rate/
cent error and owner mapping saved. Noise tone pitch Hz/cents blank; internal
KC/KF carrier initialization is not source pitch. New source music CSV includes
source rows/events/runs; zero-time causal controls remain in original ordered
trace, not falsely included as positive note members.

15 focused authored checks pass:8 new note/noise,5 performance,1 allocation,
1 force. Check cases decay/rise/zero pitch, positive collapsed target note,
source unchanged, noise owner permutation, tone restoration, conflict/force,
SSG actual7/global reg, held-noise2pass L, zero-noise ownership and EOF rest.
Initial CSV writer had extra parenthesis, fixed before runtime checks. Initial
TestCase import collected9 existing SSG tests including authored MDX/PDX helper;
helper passed; old noise-rejection test expectation legitimately updated.
Scoped imports fixed, later runs collect only intended cases. No private VGM,
compiler/listening regression. User owns real regression.

Saved authored CLI outputs under outputs/diagnostics/psg_ssg_noise_20261016.
PSG and SSG each5 source notes/rests; positive noise periods10/20 ontrackH.
PSG source/target noise rates11186.075/11363.63636 and5593.0375/5681.81818;
SSG12480/12500 and6240/6250 at current reset prescale. Source hash unchanged.
Final inferred Key-Off110250 both; final MML722/571 bytes. No compilation or
sound equivalence inferred from MML size. Architect completion review reports
no remaining blocker after fixes. README brief, fullspec docs/psg_musical_noise.md.

## User DSLAYER4 regression after checkpoint

User supplied export_mdx.py result with --no-vgm --pcm-policy best-effort
--compiler mmlx --force:17/20 exported. DSLY4_11 and14 conversion_timeout;
DSLY4_17 compilation_failed. The supplied log does not identify the internal
conversion stage or compiler error cause, so no size/time complexity cause is
asserted. No investigation/rerun or code changes performed for this report.

DSLY4_12 succeeds with PSG3 noise-owner conflict omissions210 intervals;
DSLY4_14 reports PSG2 conflict195 intervals before timeout. Each interval is
listed against both reg06/07; the two rows do not mean double lost duration.
Other supplied force summaries report none. Supported noise rate/timbre remains
approximate even when force omission count is zero. These are user export
results, not independent source/player equivalence or completed listening.
