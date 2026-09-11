# AI Business Investigator

**Know what changed. Understand why.**

AI Business Investigator is an autonomous Virtual Data Analyst that
connects to business data, verifies KPIs, investigates changes,
distinguishes mathematical drivers from unsupported root causes, and
returns evidence-backed business explanations.

## Hackathon Build

Current working stack:

-   **RocketRide** --- agent/workflow orchestration
-   **OpenAI** --- reasoning and synthesis
-   **PostgreSQL / Supabase** --- primary KPI source of truth
-   **Hotdata** --- additional analytical queries through its HTTP SQL
    API
-   **Cognee** --- business/investigation memory layer being integrated
-   **HydraDB** --- knowledge-graph storage layer for connected business
    memory
-   **Rote / Modigo** --- planned reusable investigation-procedure
    memory
-   **Snyk** --- planned repository/dependency security scanning

## Working Runtime

``` text
User
  ↓
RocketRide
  ↓
Input Guardrail
  ↓
Analyst Agent
  ├── OpenAI LLM
  ├── PostgreSQL / Supabase
  └── Hotdata HTTP API
  ↓
Readable evidence-backed business report
```

## Demonstrated Capabilities

The working prototype can:

-   query real Olist business data
-   calculate revenue, distinct orders, and AOV
-   compare historical periods
-   identify mathematical/proximate KPI drivers
-   avoid presenting unsupported root causes as facts
-   communicate uncertainty and confidence
-   return readable business reports instead of raw JSON
-   execute analytical SQL through PostgreSQL
-   execute Hotdata SQL through `POST /v1/query`

## Demo Dataset

Olist e-commerce dataset.

Primary analytical table:

`olist_summary`

Verified table grain:

**One row per order**

Verified: - 99,441 rows - 99,441 distinct `order_id` values

## KPI Definitions

### Orders

``` sql
COUNT(DISTINCT order_id)
```

### Revenue

``` sql
SUM(payment_value)
```

### AOV

``` text
Revenue / distinct orders
```

### Primary Date

`order_purchase_timestamp`

## Example Verified Investigation

May 2017 vs June 2017:

-   May revenue: \$590,703.98
-   June revenue: \$511,276.38
-   May orders: 3,695
-   June orders: 3,245
-   May AOV: \$159.83
-   June AOV: \$157.56

Conclusion:

**Lower order volume was the primary mathematical driver of the revenue
decline. Lower AOV was a smaller secondary driver. The underlying
business cause was not established by that aggregate analysis alone.**

## Hotdata Integration

Hotdata contains the Olist analytical dataset and is queried from
RocketRide using:

``` text
POST https://api.hotdata.dev/v1/query
```

The integration is used for additional diagnostic analysis such as
geography, customer behavior, and operational investigation.

A working Hotdata test retrieved June 2017 customer-state rankings from
`public.olist_summary`.

## Memory Architecture

The intended memory layer separates live facts from historical
knowledge:

``` text
Live analytical evidence
PostgreSQL + Hotdata
        ↓
RocketRide Analyst
        ↕
Cognee
        ↓
HydraDB
```

Cognee is intended to recall previous investigations and business
knowledge, while HydraDB stores connected relationships among incidents,
KPIs, drivers, periods, and findings.

## Reusable Investigation Procedures

Rote / Modigo is intended to preserve successful investigation
procedures such as:

``` text
Detect KPI change
→ verify Revenue / Orders / AOV
→ identify mathematical driver
→ investigate relevant dimensions
→ synthesize evidence
→ report confidence and uncertainty
```

This allows future investigations to reuse successful analytical
procedures rather than rediscovering them from scratch.

## Security

-   RocketRide Guardrails provide an input safety boundary.
-   Database access should use read-only credentials.
-   SQL is restricted conceptually to read-only analytical queries.
-   Secrets must never be stored in prompts or committed to GitHub.
-   Snyk is intended to scan the submitted repository and dependencies.

## Project Files

-   `guardrails.md` --- analytical/safety policy
-   `schema.md` --- approved database schema and KPI definitions
-   `output_contract.md` --- evidence and response contract
-   `architecture.md` --- hackathon stack and component responsibilities
