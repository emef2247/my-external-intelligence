# Export MDX default buffer and policy — 2026-10-30

User reverted their temporary run_batch signature edit and explicitly requested
export_mdx CLI defaults: PCM buffer1200KiB, PCM policy best-effort, no verification
VGM unless requested. Apply MDX-specific fallback after argument parsing so
legacy OPM diagnostic target routes do not inherit PCM-only options. Expose
new defaults in help. --no-vgm was already the default; --vgm stays opt-in.
Explicit buffer/policy values still override. Python run_batch API/canonical
converter defaults remain unchanged; do not equate function signature defaults
with CLI values passed explicitly into it.

1200means1200*1024=1228800bytes of resident capacity. Direct loader and default
4096-byte margin give1224704bytes of table-inclusive PDX budget. MXCTRL would
subtract a further270bytes when explicitly selected. No independent file limit
or fixed1.2MB ceiling. Converter does not configure the guest resident.

Minimal validation:34export tests pass, including CLI forwarding/defaults,
explicit strict/512KiB/--vgm overrides and inherited diagnostic failure status.
No private conversion or listening. Public docs and handoff updated; README
untouched. User owns further regression.
