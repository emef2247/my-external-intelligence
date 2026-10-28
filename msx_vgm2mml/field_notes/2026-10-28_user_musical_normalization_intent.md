# 2026-10-28: user clarification of musical normalization intent

The user clarified that normalize means decomposition into musical note values,
then discarding sub-smallest-value residue, rather than fitting tempo from OPLL
KEYON anchors. They expressed willingness to discard non-score jitter in the
musical layer because exact Segment evidence and original VGM remain available.
This is a design-direction clarification; no new lossy pass was implemented in
this turn. It does not authorize conflating short source control slices with
independent notes or deleting every SW-envelope update.

## Existing Tcl evidence

The sibling ../../msx_vgm2ms2/tcl/psg.mml.tcl get_mml_MGS (around896) decomposes
length into standard/dotted values down to the l64 unit, independent of OPLL.
The sibling tcl/mml.tcl get_normalize (1411) also snaps nearby values, including
49/47->48 and33/31->32, rather than only chopping a residual fraction.
get_quantize (1434) converts duration to l64 units and carries the time difference
in adjustment(channel) to the next interval. Thus the old algorithm does not
simply discard cumulative duration on every interval. These are distinct facts
from the current Python OPLL-based fitted-clock feature.

## Open design points, not yet implemented decisions

Interpretation to confirm before implementation: retain the64th note itself and
discard only smaller residuals, consistent with the preceding decomposition
request. The latest wording used "64th or below", so do not silently delete all
64th notes. A musical64th is tempo-dependent: shared normalization needs an
explicit common reference tempo, not an implicit per-target frame clock.

A common time-axis/grid or retained error balance avoids independently flooring
every duration and progressively changing channel alignment/song duration.
Complete sounding units and internal synthesis curves must be separated first.
Quantized score music can then feed structure/macro/envelope and MML/VGM/MIDI
backends consistently; original Segments must remain exact. State clearly which
products use native timing and which use normalized music, retaining source links
and timing-loss diagnostics. Existing tempo inference should remain a separately
identified capability, not an OPLL prerequisite for score decomposition.

No conversion code or generated user products were changed for this clarification.
The working tree still contains prior uncommitted relative/phrase-macro work.
