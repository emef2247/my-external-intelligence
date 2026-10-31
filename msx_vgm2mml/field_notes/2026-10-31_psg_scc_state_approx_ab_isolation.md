# PSG/SCC state approximation A/B isolation on Salamander

Date: 2026-10-31
Status: candidate (measured on 4 songs, current main)

## Context

The 2026-10-29 SALAMD comparison changed three things at once versus
v1.1.0 (loop mode, macro implementation with relative-state
preparation, and PSG/SCC state approximation at 24 ms with error
budgets 1 / 0.25). Which change caused the regression was never
isolated.

## What happened

Same code, same settings, only the state approximation toggled
(batch_vgm_to_mgs.py, --software-volume-curves, fm-state 24 ms,
normalization 8 ms, --force):

- SALAMD 01: outputs byte-identical with the approximation on and off
  (MML and MGS). Decision records: every note was either protected
  (hardware envelope / noise path / rest) or retained with reason
  no_bounded_simpler_observed_curve. Replaced notes: 0 (PSG and SCC).
- SALAMD 05, 08, 13 with the approximation on: replaced notes 0,
  lossy false, maximum error 0 on both chips. (08 still fails MGSC
  compilation on capacity; its curve diagnostics were produced.)

## What to do next time

With current code, the state approximation is inert on this corpus:
it cannot explain the historical accuracy or size regression. The
remaining suspects are the loop mode and the macro/relative-state
changes (or the curve implementation's state at that time). Do not
re-enable the approximation by default expecting size savings; on
these songs it demonstrably saves nothing.
