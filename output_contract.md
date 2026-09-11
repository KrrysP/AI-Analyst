# AI Business Investigator --- Output Contract

## Current Demo Output

The live RocketRide chat should return a readable Markdown business
report.

Do **not** display raw JSON to the business user.

## Preferred Report Format

``` text
# Short analytical headline

## Executive Summary
Direct answer in 2–4 sentences.

## KPI Comparison
Compact Markdown table.

## Key Drivers
Most important verified drivers in descending importance.

## What the Evidence Shows
Strongest verified findings.

## What We Still Don't Know
Important uncertainties.

## Recommended Next Step
1–3 evidence-supported actions or analyses.

## Confidence
High / Medium / Low with a short reason.

## Evidence
Concise traceability to source, period, definitions, and query summary.
```

## Formatting Rules

-   Currency: `$#,##0.00`
-   Percentages: one decimal place
-   Counts: comma separated
-   AOV: two decimal places
-   No long floating-point values
-   No raw JSON dump
-   No huge SQL blocks unless explicitly requested
-   Keep output concise and decision-oriented

## Analytical Rules

The final report must:

-   answer the original question
-   preserve requested metrics/dimensions/periods
-   distinguish mathematical drivers from root causes
-   distinguish current evidence from historical memory
-   label uncertainty
-   avoid unsupported causality
-   provide evidence-linked recommendations
-   state justified confidence

## Internal Structured Contract

For a future dashboard/frontend, the internal representation may use:

``` json
{
  "headline": "",
  "executive_summary": "",
  "kpis": [],
  "key_drivers": [],
  "confirmed_findings": [],
  "remaining_uncertainties": [],
  "recommended_actions": [],
  "confidence": "",
  "confidence_reason": "",
  "visualizations": [],
  "evidence": []
}
```

This structured form is intended for machine/frontend use, while the
current RocketRide Chat displays the readable Markdown report.
