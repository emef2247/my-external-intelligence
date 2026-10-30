# MUCOMSX Playground native SCC extension update — 2026-10-29

User supplied the official extScc Playground template. Read current upstream
MUCPlayground/readme.md, manual-data.js, NEWS.md and repository overview. Copies
of fetched public material are ignored under outputs/diagnostics/
mucomsx_chip_extension_20261029. No converter or installed runtime changed.

The current Playground is MDPlayer4Mac compiler/driver+chip emulation compiled to
WASM. Dedicated SCCA..SCCE native SCC channels are documented for msPb0102;
MucoMSX ? commands (?WD waves, ?EP envelope profiles, ?MV/?MW vibrato,
?DB detune, SCCS ?FD/?FC shared phrases, ?XU compact clock units) are compiled
into packed msPb0103..010A. NEWS associates the extension templates/repeat syntax
with MDPlayer4Mac0.2.6 on2026-10-27; newer header fixes reference0.2.7/0.2.8.

This is separate from pinned local MucoMSX_261016.zip/v1.0-stable msPb0101
SSG-to-SCC3channel extension. Do not generalize the older route's three-channel
limit to current Playground/MDPlayer native SCC output. Conversely, browser/Mac
support does not establish that the pinned DOS MUC2MUB/MUBScope tools can load
these newer containers or commands. Actual MSX runtime compatibility remains
unverified. Do not silently switch the existing SCC-to-OPNA route.

A future explicit extension target could lower native SCC waves/gates to5parts
and share phrases directly. ?EP profiles need a validated fit against observed
source curves; they cannot be assumed to represent arbitrary SW volume curves
without loss. Retain original evidence and fallback trajectory rendering.
Blueprint remains the user's requested plain MUCOM88 MUC; native chip extension
commands belong in a distinct production output profile, not its score blueprint.

Sources:
https://github.com/ToughkidDev/MUCOMSX/blob/main/MUCPlayground/readme.md
https://github.com/ToughkidDev/MUCOMSX/blob/main/MUCPlayground/manual-data.js
https://github.com/ToughkidDev/MUCOMSX/blob/main/NEWS.md
https://toughkiddev.github.io/MUCOMSX/MUCPlayground/?ex=extScc
