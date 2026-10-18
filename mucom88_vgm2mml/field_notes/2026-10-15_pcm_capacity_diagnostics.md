# Precise MUCOM PCM capacity diagnostics (2026-10-15)

User reported GFII09 MUC generation success, GFII05..08 capacity failures with
--pcm-only --pcm-policy best-effort. Read existing GFII08.mucom.json and saved
artifact listing only: old report contains external_pcm_projection/rejected
and the generic sample/address capacity message, no byte counts or per-sample
failure context. No private source conversion or encoding rerun.

Replaced the combined guard with sample_slot_count, sample_length_field,
sample_alignment and bank_payload_bytes codes. Exceptions include actual/limit,
one-based attempted slot, source interval, source IDs, candidate and existing
payload bytes. Per-run pcm_mucom_capacity.json is started before analysis and
updated through packaging, then blocked or accepted. On capacity error the
structured context and completed target chunks persist before propagation to
normal MUCOM report/batch log. No target limits/codec/audio behavior changed.

Two small authored checks passed: aggregate bank failure persists exact values;
per-sample length failure is classified independently. Inspected a persistent
synthetic bank error with candidate20 bytes against patched4-byte test limit.
This is synthetic evidence, not a measurement for GFII. Actual historical sizes
still require a user rerun with the new diagnostic code. Requested existing
OPM/OPNA investigations and implementations remain paused. Worktree only;
no stage/commit/push. Docs and handoff updated.
