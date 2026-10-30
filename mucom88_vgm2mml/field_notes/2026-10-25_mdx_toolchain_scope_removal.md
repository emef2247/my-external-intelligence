# MDX external-tool distribution scope removal (2026-10-25)

User decision: MSX output is MGSDRV; MUCOM output is MUCOM88/MUCOMSX, with
existing explicit MGS compatibility. MDX production tooling belongs to x68k.

Removed tracked MDX export/compiler/replay/fixture-generator/metadata-inspector
scripts and the Rust helper sources/Cargo dependency manifests, plus tests that
exist solely to drive those tools. Removed the corresponding mmlx, soundlog,
MXC, run68x and mdxtools third-party notice sections where present. Retained
ISC attribution for referenced mapping material still present in shared code;
this is not a claim that deleting an entry would relicense retained code.

Public CLI MDX/OPM aliases are rejected before output creation. Shared source,
structural and reference modules were retained rather than purged by filename.
Historical documents containing retired tool commands are explicitly marked.
Existing ignored local installations, private fixtures and generated products
were preserved. Main README files and x68k were not edited. No commit/push.

Validation: MUCOM: 9 focused native/MUCOMSX CLI tests passed; test discovery imported 994 tests without errors.
No private conversion, full regression or license-policy reinterpretation was
performed. The user owns regression/listening and README updates.

Additional check: public volume_sweep VGM -> inherited MGS MML succeeded with --dump-passes. MDX-only compatibility options are no longer exposed. scripts/README.md remains user-owned and contains historical MDX tool entries for the user to edit.
