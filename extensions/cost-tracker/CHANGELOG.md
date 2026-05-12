# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-05-12

### Added

- `/speckit.cost-tracker.record` command: records actual LLM spend to the spec's Cost Allocation table after each implementation step
- `/speckit.cost-tracker.report` command: renders a cross-spec budget summary table with ✓ ok / ⚠ warn / ⛔ over status per feature
- `after_implement` hook (optional, with confirmation prompt) for automatic spend recording
- Configurable warning threshold (`warn_at_pct`, default 80%)
- Configurable token pricing (`input_per_1k`, `output_per_1k`)
- Graceful degradation when Cost Allocation section is absent or budget is zero
- CI-friendly exit code (exit 1 if any feature exceeds approved budget)
