# Architecture

## Overview

Lead Generation CRM is a local-first workflow engine for collecting, normalizing, resolving, qualifying, and reviewing lead data across reusable campaigns.

The production implementation is maintained privately. This document presents the high-level architecture and engineering boundaries without exposing proprietary matching rules, scoring logic, or internal operational details.

## High-Level Workflow

```text
Campaign Configuration
        |
        v
Source Adapter Execution
        |
        v
Source Records and Evidence
        |
        v
Normalization
        |
        v
Identity Resolution
        |
        v
Qualification and Prioritization
        |
        v
Human Review and Authorization
        |
        v
Outreach and Outcome Tracking
```

## Main Subsystems

### 1. Campaign Configuration

Campaign configuration defines the operating context for a lead-generation workflow.

Responsibilities include:

- Country and region boundaries
- Industry or niche selection
- Source enablement
- Terminology and field mappings
- Zero-cost constraints
- Usage limits
- Outreach rules
- Human-approval requirements

Configuration is resolved in layers so reusable defaults can be combined with campaign-specific overrides.

### 2. Source Adapters

Source adapters convert different input formats into a common ingestion contract.

Implemented foundations include:

- CSV imports
- Manual records
- Manual URLs
- Adapter registration
- Capability declarations
- Source-run metadata
- Retry and failure reporting

Planned adapters include public registries, websites, creator platforms, job listings, and other permitted sources.

### 3. Source Runs

Each adapter execution produces a source-run record.

Source runs provide:

- Execution identity
- Adapter identity
- Campaign context
- Parameters
- Usage information
- Warnings
- Failures
- Retryability
- Result counts
- Provenance for imported records

This makes ingestion observable and auditable.

### 4. Canonical Domain Models

Data from different sources is translated into stable domain contracts.

Representative entities include:

- Campaigns
- Organizations
- Contacts
- Evidence
- Source records
- Configuration snapshots
- Identity candidates
- Match decisions
- Merge records
- Usage entries
- Outreach authorization

These contracts provide consistent inputs to persistence, identity resolution, qualification, and review workflows.

### 5. Persistence Layer

The system uses local persistence with versioned database migrations.

The persistence layer is responsible for:

- Schema lifecycle
- Repository contracts
- Transactional writes
- Stable identifiers
- Idempotent operations
- Configuration snapshots
- Source-run storage
- Merge and match history
- Usage-ledger records

Database access is kept behind repository boundaries rather than spread through application logic.

### 6. Idempotency

Repeated imports and operations should not create uncontrolled duplication.

Idempotency support includes:

- Stable operation keys
- Source-record identity
- Repeat-run detection
- Repository-enforced uniqueness
- Lifecycle tests
- Explicit conflict behavior

This is especially important for recurring imports and source monitoring.

### 7. Identity Resolution

Identity resolution determines when records from different sources refer to the same real-world organization or contact.

The architecture separates:

- Candidate generation
- Match evidence
- Decision records
- Merge execution
- Merge history
- Human-review boundaries

The system is designed to preserve evidence and avoid irreversible assumptions.

### 8. Qualification and Prioritization

The planned qualification layer will evaluate leads using evidence-backed criteria.

Potential signals include:

- Business type
- Location
- Service relevance
- Website quality
- Hiring activity
- Technology usage
- Trigger events
- Contact availability
- Evidence confidence
- Timing priority

Scoring is intended to remain inspectable rather than becoming an unexplained black box.

### 9. Human Review and Authorization

Human review is a required boundary before outreach.

The review layer is intended to support:

- Evidence inspection
- Identity confirmation
- Merge review
- Qualification review
- Contact verification
- Personalization review
- Outreach approval
- Rejection and correction

The system should surface uncertainty rather than silently fabricating missing information.

### 10. Outreach and Outcome Tracking

The planned outreach layer will support:

- Draft generation
- Approval status
- Controlled sending
- Follow-up scheduling
- Response tracking
- Outcome classification
- Campaign performance analysis

Sending is not intended to bypass authorization or platform limits.

### 11. Usage and Zero-Cost Controls

The project is designed around a strict zero-cost baseline.

Usage controls are intended to track:

- Adapter executions
- Source limits
- Free-tier allowances
- Remaining quotas
- Warning thresholds
- Blocked actions
- Retry costs
- Campaign-specific usage

The system should warn before an action exceeds configured free limits.

## Architectural Boundaries

### Adapter Boundary

Adapters may collect and normalize source-specific data, but they should not own:

- Identity decisions
- Qualification scores
- Outreach authorization
- Campaign-wide merge policy

### Identity Boundary

Identity services may propose and record matches, but they should preserve evidence and reviewability.

They should not silently discard source records or fabricate certainty.

### Persistence Boundary

Repositories own storage mechanics.

Domain and service layers should depend on repository contracts instead of embedding database-specific behavior.

### Outreach Boundary

Outreach must remain downstream of:

- Evidence capture
- Identity resolution
- Qualification
- Contact verification
- Human authorization

### Public Showcase Boundary

The public repository contains only high-level documentation and synthetic examples.

It excludes production source, private lead data, proprietary rules, internal fixtures, and operational credentials.

## Design Principles

- Local-first processing
- Reusable campaign architecture
- Explicit domain contracts
- Stable identity
- Idempotent operations
- Evidence before decisions
- Human approval before outreach
- No fabricated data
- Zero-cost baseline
- Versioned persistence
- Observable source runs
- Testable stage boundaries

## Current Status

### Implemented or Substantially Developed

- Domain contracts
- Database migrations
- Repository foundations
- Configuration resolution
- Configuration snapshots
- Zero-cost execution preflight
- Adapter protocol and registry
- CSV import lifecycle
- Manual-record ingestion
- Manual-URL ingestion
- Source-run contracts
- Idempotency
- Identity candidates and evidence
- Match decisions
- Merge foundations

### In Progress

- Additional identity-resolution behavior
- Qualification architecture
- More source adapters
- Trigger-event support
- Usage and quota workflows

### Planned

- Registry and website discovery
- Creator and platform research
- Job-posting monitoring
- Contact discovery
- Qualification scoring
- Human-review dashboard
- AI-assisted personalization
- Controlled email workflows
- Follow-up and outcome tracking

## Private Implementation Boundary

The public repository intentionally excludes:

- Application source code
- Full schema and migration definitions
- Complete identity-matching algorithms
- Scoring formulas
- Private evaluation fixtures
- Lead and contact records
- Outreach templates
- Stage builders
- Internal operational documentation
