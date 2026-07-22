# Engineering Decisions

## Overview

This document summarizes major engineering decisions made during the development of Lead Generation CRM.

The production implementation is private. The goal is to explain architectural tradeoffs, safety boundaries, and development discipline without exposing proprietary matching logic, scoring formulas, private datasets, or internal stage builders.

## 1. Local-First Operation

### Decision

Keep campaign data, lead records, identity history, and workflow state local by default.

### Why

Lead-generation systems may contain private business research, unpublished campaign plans, contact details, and sensitive operational data. Local storage reduces unnecessary exposure and keeps the user in control.

### Tradeoff

Local operation requires careful database lifecycle management, backups, portability, and packaging.

## 2. Zero-Cost Baseline

### Decision

Design the baseline system around free data sources, local computation, and explicit free-tier limits.

### Why

The system should remain usable without paid APIs or subscription dependencies.

### Tradeoff

Some sources may have stricter quotas, lower convenience, or limited coverage. The application must make those limitations visible rather than hiding them.

## 3. Human Approval Before Outreach

### Decision

Require a human-review and authorization boundary before messages are sent.

### Why

Automated outreach can create reputational, legal, and quality risks. The user must be able to inspect evidence, verify contact details, review personalization, and approve the final message.

### Tradeoff

The workflow is intentionally not a fully autonomous bulk-sending system.

## 4. No-Fabrication Boundary

### Decision

Reject or clearly mark unsupported contact details, claims, and personalization instead of inventing missing information.

### Why

False details damage trust and may produce misleading outreach.

### Tradeoff

Some leads will remain incomplete until additional evidence is found or a human supplies missing information.

## 5. Evidence-First Qualification

### Decision

Store the evidence supporting a lead decision rather than keeping only a score.

### Why

A score without supporting evidence is difficult to review, debug, or improve. Evidence enables human verification and makes prioritization explainable.

### Tradeoff

The data model and user interface must handle more detail than a simple ranked list.

## 6. Reusable Campaign Architecture

### Decision

Separate campaign configuration from core application behavior.

### Why

The same engine should support different countries, industries, services, terminology, and source combinations without requiring a new codebase for each campaign.

### Tradeoff

Configuration resolution and validation become first-class architectural concerns.

## 7. Layered Configuration

### Decision

Resolve configuration through reusable defaults, campaign settings, and controlled overrides.

### Why

Layering supports reuse while preserving campaign-specific behavior and reproducible execution.

### Tradeoff

Resolution order must be explicit, deterministic, and covered by tests.

## 8. Structured Domain Contracts

### Decision

Represent campaigns, organizations, contacts, evidence, source records, source runs, identity decisions, merges, usage, and authorization as explicit typed contracts.

### Why

Structured contracts improve:

- Validation
- Persistence
- Testing
- Serialization
- Stage boundaries
- Auditability
- Future UI integration

### Tradeoff

More schema and migration discipline is required than in an unstructured scraping script.

## 9. Adapter-Based Ingestion

### Decision

Place source-specific collection and normalization behind adapter contracts.

### Why

CSV files, manual records, manual URLs, registries, websites, and future platforms have different input and failure behavior. A shared adapter contract lets the rest of the system process them consistently.

### Tradeoff

Each adapter must satisfy shared lifecycle, warning, failure, identity, and retry expectations.

## 10. Explicit Source Runs

### Decision

Record every adapter execution as a source run with parameters, usage, warnings, failures, and result counts.

### Why

Source runs make collection observable and provide provenance for imported records.

### Tradeoff

The system stores more operational metadata, but troubleshooting and auditing become much easier.

## 11. Stable Source-Record Identity

### Decision

Use deterministic source-record identity and defined locator precedence.

### Why

Repeated imports should recognize the same source record instead of creating duplicates.

### Tradeoff

Identity rules must be carefully designed for incomplete, changing, or inconsistently formatted source data.

## 12. Idempotent Operations

### Decision

Design imports, repository writes, decisions, and lifecycle operations to be safely repeatable.

### Why

Recurring collection and interrupted workflows are normal. Re-running an operation should not corrupt state or create uncontrolled duplication.

### Tradeoff

Idempotency keys, uniqueness rules, and conflict behavior must be explicit.

## 13. Separate Candidate Generation from Identity Decisions

### Decision

Keep potential identity matches separate from accepted match decisions and merge execution.

### Why

A possible match is not the same as a confirmed identity. Preserving this distinction supports human review and prevents premature record collapse.

### Tradeoff

Identity resolution becomes a multi-stage workflow rather than a single automatic function.

## 14. Preserve Merge History

### Decision

Record merge lineage and maintain stable canonical identity.

### Why

Users must be able to understand where merged data came from and how an organization or contact reached its current state.

### Tradeoff

Merge operations require stronger persistence and lifecycle contracts.

## 15. Versioned Database Migrations

### Decision

Evolve the database through an ordered migration chain.

### Why

Versioned migrations provide a reproducible path from an empty database to the current schema and support safe future upgrades.

### Tradeoff

Every schema change requires migration design, lifecycle tests, and updates to current-schema assertions.

## 16. Repository Boundaries

### Decision

Keep database-specific behavior behind repository contracts.

### Why

Service and domain logic should not depend directly on SQL details. Repository boundaries improve testing and reduce coupling.

### Tradeoff

More interfaces and integration tests are required.

## 17. Secret-Safe Metadata

### Decision

Reject secret-like keys and credential-bearing URLs from general metadata and imported records.

### Why

Generic metadata fields can accidentally become an unsafe storage path for API keys, passwords, tokens, or private credentials.

### Tradeoff

Some source fields must be renamed, excluded, or handled through a dedicated future secret-storage boundary.

## 18. Explicit Retryability

### Decision

Model whether a failure can be retried instead of treating all errors the same way.

### Why

Temporary source failures, quota limits, invalid inputs, and permanent configuration problems require different responses.

### Tradeoff

Adapters and services must classify failures consistently.

## 19. Usage Ledger and Quota Awareness

### Decision

Track source usage and expose free-tier limits as part of execution planning.

### Why

A zero-cost promise is credible only when the system can warn or block actions that would exceed configured limits.

### Tradeoff

Quota definitions and usage accounting must be maintained per source.

## 20. Stage-Based Development

### Decision

Build the project through explicit stages with acceptance criteria and tagged checkpoints.

### Why

The system spans domain modeling, persistence, configuration, adapters, identity, scoring, outreach, and UI. Staged development makes each boundary reviewable and protects earlier work.

### Tradeoff

The process is more formal than rapid prototyping, but regressions and architectural drift are less likely.

## 21. Full-Suite Acceptance

### Decision

Require the complete project suite for major stage acceptance, not only focused tests.

### Why

A change can satisfy its own tests while breaking migration assumptions, repository behavior, or earlier stages.

### Tradeoff

Acceptance runs take longer, but confidence is materially higher.

## 22. Actual-Tree Builder Preflight

### Decision

Require builders and patchers to inspect the real repository tree before writing files.

### Why

Assuming a clean skeleton can overwrite existing package markers, conflict with implemented namespaces, or miss current tests and migrations.

### Tradeoff

Builder scripts require more preflight logic, but destructive assumptions are avoided.

## 23. Private Production Source

### Decision

Keep the complete implementation private and maintain a separate public showcase repository.

### Why

This protects proprietary work and private operational details while still allowing employers to evaluate:

- Architecture
- Testing discipline
- Safety boundaries
- Product direction
- Development status
- Engineering decisions

### Tradeoff

Complete code review requires controlled private access during a hiring process.

## 24. Honest Work-in-Progress Status

### Decision

Describe the project publicly as active development rather than presenting it as a finished commercial CRM.

### Why

The backend foundations are substantial, but qualification, outreach, monitoring, and the end-user dashboard are not yet complete.

### Tradeoff

The public showcase contains fewer product screenshots today, but remains accurate and credible.

## Future Decisions

Planned or potential future decisions include:

- Qualification-score representation
- Trigger-event evidence models
- Contact-discovery confidence rules
- Email-provider integration boundaries
- Follow-up scheduling
- Dashboard framework selection
- Campaign analytics
- Packaging and backup workflows
- Source-specific legal and platform-policy controls
