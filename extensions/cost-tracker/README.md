# spec-kit-cost-tracker

An extension for [Spec Kit](https://github.com/github/spec-kit) that tracks actual LLM spend against the approved budget in each spec's Cost Allocation section.

## Features

- **Record spend**: After each implementation step, records actual LLM cost back to the spec's `## Cost Allocation` table
- **Budget warnings**: Emits warnings at 80% of approved budget; errors at 100%
- **Cross-spec report**: Renders a summary table of budget vs. actual spend across all specs in the project
- **CI-friendly**: `/speckit.cost-tracker.report` exits 1 if any feature is over budget

## Installation

```bash
specify extension add cost-tracker --from https://github.com/duksh/spec-kit-cost-tracker/archive/refs/tags/v1.0.0.zip
```

## Configuration

Copy `config-template.yml` to `.specify/extensions/cost-tracker/cost-tracker-config.yml` and customize:

```yaml
# Percentage of approved budget at which a warning is emitted (default: 80)
warn_at_pct: 80

# Currency symbol used in output (display only)
currency: "USD"

# Token pricing used when converting token counts to USD
token_pricing:
  input_per_1k: 0.00025   # USD per 1,000 input tokens
  output_per_1k: 0.00125  # USD per 1,000 output tokens
```

## Usage

### Record LLM Spend

Run after an implementation step to update the spec's Actual LLM Spend field:

```
/speckit.cost-tracker.record
```

When installed, this also runs automatically as an `after_implement` hook (optional — you are prompted each time).

### Budget Report

Show a summary table across all specs in the project:

```
/speckit.cost-tracker.report
```

Example output:

```
┌─────────────────────────────────────────────────────────────────────────┐
│  LLM Cost Report                                   2026-01-15 14:30 UTC │
├──────────────────────┬───────────┬──────────┬────────┬───────┬──────────┤
│ Feature              │ Priority  │ Approved │ Actual │ % Used│ Status   │
├──────────────────────┼───────────┼──────────┼────────┼───────┼──────────┤
│ add-login            │ P1        │  $10.00  │  $7.80 │  78%  │ ✓ ok     │
│ dark-mode            │ P2        │   $5.00  │  $4.10 │  82%  │ ⚠ warn   │
│ data-export          │ P3        │   $3.00  │  $3.50 │ 117%  │ ⛔ over  │
├──────────────────────┼───────────┼──────────┼────────┼───────┼──────────┤
│ TOTAL                │           │  $18.00  │ $15.40 │  86%  │ ⚠ warn   │
└──────────────────────┴───────────┴──────────┴────────┴───────┴──────────┘
```

## Spec Template

Add a `## Cost Allocation` section to your spec:

```markdown
## Cost Allocation

| Field | Value |
|-------|-------|
| **Team** | platform |
| **Cost Center** | eng-platform-001 |
| **Feature Priority** | P2 |
| **Approved LLM Budget (USD)** | $10.00 |
| **Actual LLM Spend (USD)** | [populated by Cost Tracker extension] |
| **Clarification Budget** | 3 |
```

## Requirements

- Spec Kit `>=0.7.2`

## License

MIT
