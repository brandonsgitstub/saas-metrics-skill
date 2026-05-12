---
name: saas-metrics-analyzer
description: Turn raw SaaS metrics (MRR, churn, growth, pricing tiers) into a complete financial analysis with scenario modeling, stage-appropriate industry benchmarks, and prioritized growth recommendations. Use when the user provides SaaS metrics or asks for a health check, deep dive, board deck prep, fundraising prep, or financial model.
---

# SaaS Metrics Analyzer

A skill for turning raw SaaS metrics into a complete financial analysis with scenario modeling, industry benchmarks, and actionable growth recommendations.

This skill takes whatever metrics you have — from a simple MRR number to a full breakdown with cohort data — and produces a diagnostic that tells you where you stand, where you're headed, and what to do about it.

---

## Operating Principles

When analyzing SaaS metrics, follow these principles:

- **Think in components, not averages.** Break MRR into new, expansion, contraction, and churned. Averages hide problems.
- **Always show the math.** Every projection should include the formula and assumptions behind it. No black boxes.
- **Flag vanity metrics.** If a user provides gross revenue instead of net, or counts free users in subscriber counts, call it out.
- **Benchmark against stage, not unicorns.** A 3% monthly churn is fine for an early-stage SMB product. Don't compare it to Snowflake.
- **Be direct about bad news.** If the metrics show the business is shrinking, say so clearly. Sugarcoating wastes everyone's time.

---

## Phase 1: Classify and Route

Read the user's input and classify into one of two tiers:

### Tier 1 — Quick Analysis

**Trigger:** User provides fewer than 4 metrics, asks for a "quick look" / "health check" / "sanity check", or provides only top-line numbers without detail.

**Behavior:** Skip the interview. Produce a focused diagnostic immediately. Output as Markdown in the chat.

### Tier 2 — Full Financial Analysis

**Trigger:** User provides 4+ metrics, mentions fundraising / investors / board deck, asks for scenarios or projections, provides tier-level or cohort-level data, or asks for a "full analysis" / "deep dive" / "financial model".

**Behavior:** Run a brief structured interview (Phase 2), then produce a comprehensive `.docx` report.

---

## Phase 2: Gather Context (Tier 2 Only)

Ask 3–5 targeted questions to fill gaps. Only ask what's missing — if the user already provided the data, skip that question. Frame it conversationally: *"A few quick questions so I can build a useful analysis — skip any you don't know."*

**Question categories** (ask only what's needed):

1. **Metrics snapshot** — What's your current MRR? How many paying customers? What's your monthly churn rate? Monthly growth rate?
2. **Revenue dynamics** — Do you see expansion revenue (upgrades, add-ons)? What about contraction (downgrades)? How are your pricing tiers structured?
3. **Unit economics** — What's your CAC? Average sales cycle? Gross margin? *(Mark `[NOT PROVIDED]` if the user doesn't know — don't block on this.)*
4. **Business context** — What stage are you? (pre-seed / seed / Series A / Series B+) What's your primary customer segment? (SMB / mid-market / enterprise) Primary acquisition channel?
5. **Analysis goal** — What decision is this analysis informing? (fundraising prep, board update, pricing change, resource allocation, general health check)

If the user says "just draft it" or "I don't have that", proceed with what you have. Mark every gap with `[NOT PROVIDED — analysis would improve with this data]` rather than inventing numbers.

---

## Phase 3: Analyze and Produce Output

### Computed Metrics

Calculate all of the following from the provided data. If a required input is missing, note what's needed rather than guessing.

| Metric | Formula | When to Calculate |
|--------|---------|-------------------|
| **ARPU** | MRR / total paying customers | Always |
| **LTV** | ARPU / monthly churn rate | When churn is provided |
| **Net Revenue Retention (NRR)** | (Starting MRR + Expansion − Contraction − Churn) / Starting MRR × 100 | When expansion/contraction data exists |
| **SaaS Quick Ratio** | (New MRR + Expansion MRR) / (Churned MRR + Contraction MRR) | When MRR components are available |
| **LTV:CAC Ratio** | LTV / CAC | When both LTV and CAC are available |
| **CAC Payback Period** | CAC / (ARPU × gross margin) | When CAC and gross margin are available |
| **Gross MRR Churn** | Churned MRR / Starting MRR × 100 | When churned MRR is available |
| **Net MRR Growth** | Net New MRR / Starting MRR × 100 | Always |
| **Implied ARR** | MRR × 12 | Always |
| **Months to milestone** | Months to reach $100K, $500K, $1M, $5M ARR at current trajectory | Always |

### Scenario Modeling

Project 12-month and 24-month outcomes under three scenarios. Adjust variance bands based on company maturity:

| Scenario | Early Stage (<$1M ARR) | Growth Stage ($1–10M) | Scale ($10M+) |
|----------|----------------------|----------------------|----------------|
| **Bear** | Churn +40%, growth −40% | Churn +25%, growth −30% | Churn +15%, growth −20% |
| **Base** | Current trajectory | Current + slight improvement | Current trajectory |
| **Bull** | Churn −30%, growth +40% | Churn −20%, growth +25% | Churn −10%, growth +15% |

For each scenario, project month-by-month:
- Starting customers + new customers − churned customers = ending customers
- Starting MRR + new MRR + expansion MRR − churned MRR − contraction MRR = ending MRR
- Show the path to the next ARR milestone

### Industry Benchmarks

Rate each metric against stage-appropriate benchmarks. Use these reference ranges:

**Early Stage (< $1M ARR)**
| Metric | 🟢 Strong | 🟡 Typical | 🔴 Concern |
|--------|-----------|-----------|------------|
| Monthly churn | < 5% | 5–7% | > 7% |
| NRR | > 100% | 90–100% | < 90% |
| LTV:CAC | > 2:1 | 1–2:1 | < 1:1 |
| Quick Ratio | > 3 | 2–3 | < 2 |
| MRR growth rate | > 15%/mo | 10–15%/mo | < 10%/mo |
| CAC Payback | < 18 months | 18–24 months | > 24 months |

**Growth Stage ($1M–$10M ARR)**
| Metric | 🟢 Strong | 🟡 Typical | 🔴 Concern |
|--------|-----------|-----------|------------|
| Monthly churn | < 3% | 3–5% | > 5% |
| NRR | > 110% | 100–110% | < 100% |
| LTV:CAC | > 3:1 | 2–3:1 | < 2:1 |
| Quick Ratio | > 4 | 3–4 | < 3 |
| MRR growth rate | > 10%/mo | 5–10%/mo | < 5%/mo |
| CAC Payback | < 12 months | 12–18 months | > 18 months |

**Scale Stage ($10M+ ARR)**
| Metric | 🟢 Strong | 🟡 Typical | 🔴 Concern |
|--------|-----------|-----------|------------|
| Monthly churn | < 1.5% | 1.5–2.5% | > 2.5% |
| NRR | > 120% | 110–120% | < 110% |
| LTV:CAC | > 4:1 | 3–4:1 | < 3:1 |
| Quick Ratio | > 4 | 3–4 | < 3 |
| MRR growth rate | > 5%/mo | 2–5%/mo | < 2%/mo |
| CAC Payback | < 8 months | 8–12 months | > 12 months |

---

## Output Format

### Tier 1 Output — Markdown (in chat)

```
## SaaS Health Check

### Metrics Summary
[Table of provided metrics with computed values]

### Health Scorecard
[Each metric rated 🟢/🟡/🔴 against stage-appropriate benchmarks with one-line explanation]

### 12-Month Projection (Base Case)
[Month-by-month table: Month | MRR | Customers | Net New MRR]

### Top 3 Recommendations
[Numbered, specific, actionable — tied to the biggest metric gaps]
```

### Tier 2 Output — `.docx` Report

Generate a formatted Word document using the docx skill. Include the following sections:

1. **Executive Summary** — 3 sentences: where you are, where you're headed, what to do about it. No fluff.

2. **Current Metrics Snapshot** — Table of all provided and computed metrics. Side-by-side with stage-appropriate benchmark ranges. Color-code: green (strong), yellow (typical), red (concern).

3. **Health Scorecard** — Each metric gets a rating and a one-paragraph explanation of what it means for this specific business. Don't just define the metric — interpret it.

4. **Revenue Composition** — Break down MRR by tier (if tier data provided). Show the new/expansion/churn/contraction waterfall. Identify which components are driving growth or drag.

5. **Scenario Projections** — 12-month and 24-month tables for bear/base/bull cases. Include:
   - Month-by-month MRR and customer count
   - ARR milestone dates for each scenario
   - Key inflection points (when does the bear case go negative? When does the bull case hit $1M ARR?)

6. **Unit Economics Analysis** — LTV, CAC, payback period, gross margin analysis. If CAC isn't provided, calculate what the maximum viable CAC would be at a 3:1 LTV:CAC target. If gross margin isn't provided, note the impact: "At 70% gross margin, your CAC payback would be X months. At 80%, it would be Y."

7. **Benchmark Comparison** — Stage-matched benchmarks with specific gaps identified. Rank gaps by severity. For each red metric, explain why it matters and what improving it by a specific amount would mean for the business.

8. **Risk Factors** — Top 3–5 risks surfaced by the metrics. Be specific:
   - BAD: "Churn is high"
   - GOOD: "At 7% monthly churn, you're replacing your entire customer base every 14 months. This means growth is masking a retention problem that will compound as you scale."

9. **Recommendations** — 5–7 specific, prioritized actions tied to the largest metric gaps. Each recommendation should include:
   - What to do
   - Why (which metric it addresses)
   - Expected impact (quantified where possible)
   - Suggested timeline

10. **Assumptions Log** — Every assumption made during the analysis, marked `[ASSUMED]`. What data point would refine each assumption. This section exists so the reader knows exactly where the analysis is soft.

---

## Phase 4: Review Loop

After delivering the output:

1. Ask: *"Does this capture your situation accurately? Anything I'm missing or got wrong?"*
2. If the user provides corrections or additional data, re-run the affected calculations and update the document.
3. If a section is thin because data was missing, flag it: *"The unit economics section is limited because I don't have your CAC. If you can share that, I'll add LTV:CAC analysis and payback projections."*

---

## Edge Cases

- **User provides only MRR**: Calculate ARPU only if customer count is also given. Otherwise, note it as required for deeper analysis. Still produce a Tier 1 output with available data.
- **User provides a spreadsheet or CSV**: Parse it. Look for columns that map to standard metrics. Ask for clarification if column names are ambiguous.
- **Fundraising context**: If the user mentions fundraising, emphasize metrics VCs care about: NRR, LTV:CAC, burn multiple, growth rate, gross margin. Frame projections in terms of runway and milestones.
- **Negative growth**: If the business is shrinking, don't hide it. The bear case should model accelerating decline. Recommendations should focus on stabilization before growth.
- **Very early stage (< $10K MRR)**: Benchmarks are less meaningful at this scale. Focus the analysis on unit economics and product-market fit signals rather than growth benchmarks. Note that the sample sizes for churn calculations may be too small to be statistically meaningful.
