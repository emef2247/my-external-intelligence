# Ultra-low derived PCM and batch timeout (2026-10-17)

User requested smaller quarter-rate PCM option and batch default timeout600.
Added --pcm-profile ultra_low to canonical MUC CLI and export_mucom wrapper.
Shared profile nominal15625/4Hz, MDX clock4MHz/div1024/F0/chunk32s; MUCOM uses
only nominal profile.rate and its existing per-call nearest Delta-N projection.
MUCOM Delta-N4615, actual rate1499875/384Hz approximately3905.924479167Hz.
The exact rational rate remains recorded by pcm_mucom; no global mutation.
Native OPNA ADPCM-B remains unaffected/inapplicable. Source IR retained,
capacity32slots/256KiB/alignment/force policies unchanged. Low profiles still
use existing linear interpolation without dedicated anti-alias filtering;
size reduction does not guarantee runtime PCM buffer fit.

Changed export_mucom run_batch signature/parser default600 seconds per
conversion/compilation stage. Other subprocess/compiler helper default values
unchanged. Explicit --timeout still controls the batch stages. README and
focused interoperability docs updated; no private input inspection/conversion.

Saved before checks. Focused authored profile tests3/3 pass, covering quarter
payload bound, computed Delta-N and MUC note/detune spelling, Unit identity,
standard-after-ultra stability, MSM/Sega adapters, native OPNA MUC/bank exact
nonapplication, canonical and batch profile forwarding, parser/signature600.
No compiler or listening. No git mutation. Root coordinates x68k extension and
final exact shared module synchronization; user owns private regression.
