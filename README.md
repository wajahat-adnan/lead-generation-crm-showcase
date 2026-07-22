# Lead Generation CRM

A local-first, reusable lead-research and outreach workflow engine built around structured evidence, stable identity, human approval, and strict zero-cost operating constraints.

> **Project status:** Active development. The domain, persistence, configuration, adapter, idempotency, and identity-resolution foundations are implemented. Qualification, trigger monitoring, outreach workflows, and the end-user dashboard are still in development.

> This repository is a public engineering case study. The production source code is maintained privately because it contains proprietary implementation details.

## Project Overview

Lead Generation CRM is being designed as a reusable system for researching, importing, organizing, qualifying, and eventually contacting leads across different campaigns and industries.

The first use case is lead discovery for a local radiology-reporting application, but the architecture is intentionally reusable for:

- Video-editing services
- AI automation services
- Local software products
- Country-specific business outreach
- Other evidence-driven lead-generation campaigns

The system emphasizes:

- Local-first operation
- Zero-cost baseline workflows
- Human approval before outreach
- Evidence-backed qualification
- Stable record identity
- No fabricated contact or personalization data
- Reusable adapters and campaign configuration

## Current Capabilities

### Domain and Contract Models

The private implementation includes structured models for:

- Campaigns
- Organizations
- Contacts
- Evidence
- Source records
- Source runs
- Configuration snapshots
- Identity matches and merges
- Outreach authorization
- Usage and quota records

### Persistence and Migrations

- SQLite-backed local persistence
- Versioned migration chain
- Repository contracts
- Stable schema-version handling
- Lifecycle and integration tests
- Idempotent write behavior

### Configuration System

- Layered campaign configuration
- Reusable campaign templates
- Country and niche separation
- Configuration snapshots
- Zero-cost preflight checks
- Secret-safe metadata validation
- Explicit execution boundaries

### Reusable Source Adapters

Implemented adapter foundations include:

- CSV imports
- Manual records
- Manual URLs
- Adapter registry and protocol contracts
- Source-run metadata
- Retry and failure reporting
- Stable source-record identity
- Idempotent re-import behavior
- Secret-like field rejection

### Identity Resolution

- Canonical identity models
- Stable identifiers
- Match evidence
- Match decisions
- Merge lifecycle support
- Idempotency protection
- Cross-source organization handling
- Explicit acceptance boundaries

### Safety and Human Control

The architecture includes:

- No-fabrication rules
- Human approval before outreach
- Evidence and confidence tracking
- Secret-safe configuration handling
- Local data boundaries
- Zero-cost execution guards
- Usage-ledger foundations

## Planned Workflow

```text
Campaign Configuration
        |
        v
Source Discovery and Import
        |
        v
Normalization and Evidence Capture
        |
        v
Identity Resolution and Deduplication
        |
        v
Qualification and Priority Scoring
        |
        v
Human Review and Approval
        |
        v
Personalized Outreach Drafting
        |
        v
Sending, Follow-Up, and Outcome Tracking
```

## Engineering Highlights

- Adapter-based ingestion architecture
- Canonical domain contracts
- Migration-backed persistence
- Stable identity and idempotent imports
- Evidence-first qualification design
- Explicit source-run and retry models
- Human approval boundaries
- Zero-cost execution constraints
- Secret-safe configuration validation
- Extensive automated regression and acceptance testing

## Technology

The private implementation uses:

- Python
- SQLite
- Pytest
- Ruff
- mypy
- Dataclasses and typed domain models
- Versioned database migrations
- PowerShell-based validation and stage builders
- Git and staged acceptance tags

## Development Status

### Implemented or Substantially Developed

- Stage 0 project and architecture foundations
- Stage 1 domain contracts
- Stage 2 persistence
- Stage 3 configuration
- Stage 4 adapter foundations
- CSV import adapter
- Manual-record adapter
- Manual-URL adapter
- Reusable adapter behavioral contracts
- Stage 5 identity foundations
- Identity candidates, evidence, merges, and match decisions
- Automated unit, integration, contract, and acceptance testing

### In Progress

- Further identity-resolution work
- Expanded source adapters
- Qualification and scoring
- Trigger-event monitoring
- Usage and quota workflows

### Planned

- Public-registry and website discovery
- YouTube and creator research
- Job-posting and intent-signal monitoring
- Evidence-based lead scoring
- Contact discovery
- AI-assisted personalization with no-fabrication checks
- Human-approved email workflows
- Follow-up and outcome tracking
- Local dashboard and campaign review interface

## Testing

The private repository contains hundreds of automated tests covering:

- Domain contracts
- Configuration resolution
- Database migrations
- Repository behavior
- Adapter protocols
- CSV and manual import lifecycles
- Idempotency
- Identity resolution
- Merge behavior
- Zero-cost execution rules
- Full-stage acceptance

Exact counts change as development continues, so this public case study focuses on testing structure rather than a fixed total.

## Source Availability

The complete production source code is maintained in a private GitHub repository.

This public repository intentionally excludes:

- Production source code
- Full database schemas and migrations
- Complete matching and scoring logic
- Internal test fixtures
- Stage-builder scripts
- Private campaign data
- Lead records
- Contact details
- Outreach templates
- Internal operational documentation

Selected implementation details may be reviewed privately during a technical interview or hiring process.

## My Contribution

I designed and implemented work across:

- Domain modeling
- Persistence architecture
- Migration design
- Repository contracts
- Configuration resolution
- Adapter protocols
- CSV and manual import workflows
- Idempotency
- Identity resolution
- Safety and evidence boundaries
- Automated testing and acceptance workflows
- Local-first and zero-cost product strategy

## Disclaimer

This repository presents an engineering portfolio case study for an in-development system. It is not a finished commercial CRM or autonomous outreach product.
