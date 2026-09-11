# AI Business Investigator --- Hackathon Architecture

## Product Thesis

Traditional dashboards tell businesses **what happened**.

AI Business Investigator is designed to help determine **why it
happened**, using verified data and explicit uncertainty.

> **Know what changed. Understand why.**

## Current Working Core

``` text
User
 ↓
RocketRide
 ↓
Input Guardrail
 ↓
Analyst Agent
 ├── OpenAI
 ├── PostgreSQL / Supabase
 └── Hotdata HTTP API
 ↓
Readable evidence-backed report
```

## Component Responsibilities

### RocketRide

Primary orchestration layer.

Coordinates the user request, guardrails, analyst reasoning, tool calls,
and final response.

### OpenAI

Reasoning and synthesis layer.

Interprets the business question, plans useful analysis, chooses tools,
and synthesizes verified evidence.

### PostgreSQL / Supabase

Primary KPI source of truth.

Used for: - revenue - distinct orders - AOV - period comparisons -
primary verified calculations

### Hotdata

Additional analytical infrastructure.

The Olist dataset is loaded into Hotdata and can be queried through:

``` text
POST /v1/query
```

Used for diagnostic investigations such as: - geography - customer
behavior - operations/delivery - independent analytical slices

Hotdata has been successfully called from the analyst through
RocketRide's HTTP tool.

### Cognee

Business/investigation memory layer.

Purpose: - remember previous investigations - recall known findings -
provide historical business context - answer "what have we learned
before?"

Cognee memory must be distinguished from current verified data.

### HydraDB

Knowledge-graph storage layer.

Intended to preserve relationships such as:

``` text
Investigation
→ ANALYZED → KPI
→ FOUND_DRIVER → Finding
→ OCCURRED_IN → Period
```

HydraDB relationships provide context, not current KPI proof.

### Rote / Modigo

Reusable investigation-procedure layer.

Intended to preserve successful procedures such as:

``` text
Verify KPI
→ compare Revenue / Orders / AOV
→ identify mathematical driver
→ investigate relevant dimensions
→ synthesize evidence
→ report uncertainty/confidence
```

This lets repeated investigations reuse proven analytical procedures.

### Snyk

Security layer for the submitted repository/application.

Used to scan source/dependencies and demonstrate security awareness
around an agent that can execute data tools.

## Memory vs Procedure Memory

These are deliberately different:

**Cognee / HydraDB** \> What has the business learned before?

**Rote** \> How did the analyst successfully investigate this type of
problem before?

## Demonstrated Investigation

May 2017 → June 2017:

-   Revenue declined from \$590,703.98 to \$511,276.38
-   Orders declined from 3,695 to 3,245
-   AOV declined from \$159.83 to \$157.56
-   Lower order volume was the primary mathematical driver
-   Lower AOV was a secondary driver
-   Underlying root cause remained unverified from the aggregate KPI
    query

## Demo Narrative

> Dashboards are good at telling businesses what happened, but someone
> still has to investigate why. AI Business Investigator connects
> directly to business data, verifies KPIs, investigates the strongest
> measurable drivers, distinguishes evidence from hypotheses, and
> explains what remains uncertain. RocketRide orchestrates the analyst,
> PostgreSQL provides primary KPI truth, Hotdata enables additional
> analytical investigation, and the memory stack is designed to preserve
> what the business and analyst learn over time.

## Security / Secrets

Never commit: - `.env` - API keys - database passwords - Hotdata
tokens - Cognee credentials - HydraDB credentials

Use `.gitignore` and secure credential fields.
