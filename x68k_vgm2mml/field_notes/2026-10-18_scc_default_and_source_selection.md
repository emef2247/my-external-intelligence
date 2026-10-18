# SCC default, automatic bank and source-only selection (2026-10-18)

User split a private mixedPSG/SCC input into6syncblocks. All6 generated for
additive, additive8, FM+additive and FM+FM withbank; listeningpreferredFM+additive.
InitialFMattemptswithoutbankfailed as expected under previousexplicitbankguard.
Earlier whole-song exportedCSV failures were MDXtrack-start0xfffe offset limit,
not channelallocation exhaustion. Readexistinglogs only, no private reruns.

Per user, canonicaldefaultnowfm-additive. Missingbankautomaticallyfits selected
soundingSCCwaves; sameoutput keeps .scc.waveforms.csv and .scc.voice_bank.json.
Content/core/gain/clock/version cacheunder.tools/scc-fm-fit/cache. Explicitbank
stillworks. PSG-only doesnotfitSCC. Legacyopm-additivealias retainsadditive.
Source-only hyphen/underscorealiasesexclusive; nativePSG/SCC sourcesretained,
onlytargetselectionchanges. JSON records omittedsource by explicitchoice.

Changes saved beforechecks.5authoredsource/voicecasespass; public-derivedmixed
fixture (publicSCCvolumesweep+authoredAYtone) bothselectorsMMLpass, sourceCSVs
bothretained. SCC-only defaultFM/no-bank listeningMDXexportmmlxpass. NoGitmutation.
Firstprioritycheckpointcomplete; nextuserauthorizedPSG->PCM continuoussource
runtime design (sharednoise/EG, orderedwrites once), existingwaveformpackaging.
