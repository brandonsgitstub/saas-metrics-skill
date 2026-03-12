# SaaS Metrics Analyzer

A Claude Code skill that turns your SaaS metrics into a complete financial analysis with scenario modeling, industry benchmarks, and actionable growth recommendations.

Built by [Cadboro Labs](https://cadborolabs.com) — a product studio that builds AI-powered tools for SaaS builders.

## What It Does

Give it your metrics — MRR, churn, growth rate, pricing tiers — and it produces a diagnostic that tells you where you stand, where you're headed, and what to do about it.

**Quick analysis** (basic input): Health scorecard + 12-month projection + top 3 recommendations. Delivered as Markdown in your terminal.

**Full analysis** (detailed input): Comprehensive `.docx` report with scenario modeling (bear/base/bull), industry benchmarks by stage, revenue composition breakdown, risk factors, and prioritized recommendations.

## Install

```bash
claude skill install saas-metrics-skill
```

## What You Get

### Quick Analysis (Tier 1)
- Metrics summary with computed values (LTV, NRR, Quick Ratio, etc.)
- Health scorecard — each metric rated against stage-appropriate benchmarks
- 12-month base case projection
- Top 3 actionable recommendations

### Full Financial Analysis (Tier 2)
- Executive summary
- Current metrics snapshot with benchmark comparison
- Health scorecard with interpretive commentary
- Revenue composition and MRR waterfall
- 12- and 24-month scenario projections (bear/base/bull)
- Unit economics analysis (LTV, CAC, payback)
- Stage-matched benchmark comparison with gap analysis
- Risk factors with specific impact quantification
- 5-7 prioritized recommendations
- Assumptions log

## Metrics Computed

| Metric | Formula |
|--------|---------|
| ARPU | MRR / paying customers |
| LTV | ARPU / monthly churn rate |
| NRR | (Starting MRR + Expansion - Contraction - Churn) / Starting MRR |
| Quick Ratio | (New + Expansion MRR) / (Churned + Contraction MRR) |
| LTV:CAC | LTV / CAC |
| CAC Payback | CAC / (ARPU x gross margin) |

## Benchmarks

Metrics are rated against stage-appropriate ranges:

| Stage | Monthly Churn | NRR | LTV:CAC | Quick Ratio |
|-------|--------------|-----|---------|-------------|
| Early (<$1M ARR) | 5-7% typical | 90-100% | 2:1+ | >2 |
| Growth ($1-10M) | 3-5% typical | 100-110% | 3:1+ | >3 |
| Scale ($10M+) | 1-2% typical | 110-130% | 4:1+ | >4 |

## Try the Free Calculator

Not ready for a full analysis? Try our [MRR Calculator](https://cadborolabs.com/tools/mrr-calculator/) — a free interactive tool with advanced SaaS metrics, benchmark comparisons, and MRR component breakdowns.

## License

GPL-3.0
