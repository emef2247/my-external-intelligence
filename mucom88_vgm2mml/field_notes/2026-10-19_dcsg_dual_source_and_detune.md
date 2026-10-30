# Dual DCSG source analysis and detune observations

Date: 2026-10-19. User explicitly requested read-only musical analysis of the
Wonder Boy in Monster Land Sega System 2 reference while developing DCSGx2
source support. No private MUC conversion, compilation or listening regression
was performed here. Public/native authored tests are separate.

## Method and inherited decisions

Read the MSX field notes on exact PSG/SCC period retention (2026-09-29) and
OPLL source key/volume/envelope distinction (2026-10-01). Target detune must
preserve measured source period identity; control slices and volume recovery
are not automatically hardware attacks. DCSG has no KeyOn register.

Decoded all 20 `MSLDAC*.vgm` inputs in
`mucom88_vgm2mml/tests/fixtures/local_only/2xSN76489A/vgmrips.net/MSLANDAC`.
Shared `command_times` supplies exact 44100 Hz sample timestamps. Analysis is
one stream pass, no expanded loop. Each chip retains its own latch, three
10-bit tone periods, four attenuations and noise-control history. Unobserved
register bits remain unknown, so startup guesses cannot create matching notes.

The inspected headers have 2,000,000 Hz clock, dual-chip bit, mono TI-style
flags 0x05, feedback 0x000c and shift width 17. These header facts do not prove
a particular physical LFSR implementation. The decoder preserves all raw flags.

Intersected audible tone intervals between the two chips. For each song,
selected the fixed three-channel permutation maximizing total overlap within
100 cents. This is a descriptive pairing heuristic, not proof of compositional
voice identity. Many intervals are exact unisons, independent parts or
accidental note crossings; matching channel numbers alone is not reliable.

The aggregate close-pitch overlap is 24,802,893 voice-samples, of which
4,550,025 have a nonzero period difference. These sums overlap across channels
and are not song duration. Exact period equality accounts for 20,252,868.

## Coherent detuned-unison examples

MSLDAC05 chip 0 tone 0 versus chip 1 tone 2 has approximately 100% close-pitch
coverage. Restricting to nonzero differences within 30 cents and individual
stable control slices at least 735 samples yields 12.75 voice-seconds:
weighted p10 / median / p90 = -15.34 / -10.24 / -6.84 cents. Median signed beat
frequency is -0.973 Hz. Representative period pairs include 337/339, 337/340,
675/678 and 450/452. This is a slightly lower companion voice, not a fixed
integer period difference or one globally fixed cents offset.

MSLDAC12 has the same coherent channel pairing, approximately 100% close-pitch
coverage. The corresponding stable nonzero subset is 2.56 voice-seconds,
p10 / median / p90 = -15.34 / -11.50 / -7.68 cents, median beat -1.157 Hz.

MSLDAC13 shows substantial nonzero overlap but mixed detune signs and dynamic
pitch relationships; a single fixed pairing does not establish one chorus law.
MSLDAC01/06/15 contain shorter near-unison portions. Several songs have no
meaningful near-unison overlap with this pairing criterion.

Conclusion: this reference supports selectable subtle unison doubling, roughly
around 10 cents in the coherent examples, but does not justify imposing a
fixed -10-cent setting on every input or duplicating all source voices.
Preserve original musical parts and offer any future enhancement explicitly.
The reference also uses exact unisons and independent lines.

Detailed private measurement JSON is in ignored
`outputs/diagnostics/dcsg_detune_source/MSLANDAC.source_detune.json`.
No private register/waveform data or source-derived MML was added to public docs.

## Implementation boundary

`py/dcsg.py` stores native source writes, snapshots and all four channels per
chip. `py/dcsg_music.py` creates derived PSG-like musical views and reuses the
MSX extract_notes/pre-envelope source loop planner. One AY engine per chip
retains all three tones; noise prefers a free tone slot, or explicitly shares
the closest nominal gain when all three tones sound. AY cannot independently
reproduce a fourth volume. Native DCSG Segments remain unchanged.
Noise LFSR/periodic-noise differences, noise range clamp, shared gain, and
unknown/DC tone periods remain interval-linked diagnostics. The nominal
DCSG2dB-to-AY3dB volume mapping is an explicit approximation, not calibrated
hardware equivalence. No target round-trip certification is claimed by the
source/view tests.
