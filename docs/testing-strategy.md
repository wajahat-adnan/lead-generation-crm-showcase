# Testing Strategy

## Overview

Lead Generation CRM uses layered automated testing to protect domain contracts, persistence behavior, adapter lifecycles, idempotency, identity resolution, configuration safety, and zero-cost execution boundaries.

The production test suite is maintained in the private repository. This document describes the testing approach without exposing proprietary fixtures, internal stage builders, private campaign data, or complete matching logic.

## Testing Goals

The testing strategy is designed to verify that:

- Domain models reject invalid states
- Database migrations remain consistent
- Repository operations preserve contract behavior
- Repeated imports remain idempotent
- Source adapters expose predictable warnings and failures
- Identity evidence and decisions remain auditable
- Merge operations preserve lifecycle guarantees
- Secret-like data is rejected from unsafe fields
- Zero-cost execution rules cannot be bypassed silently
- New stages do not regress established behavior

## Test Layers

### Unit Tests

Unit tests focus on deterministic behavior in small components.

Representative coverage includes:

- Domain-model validation
- Identifier generation
- Serialization and deserialization
- Configuration normalization
- Secret-safe metadata checks
- Adapter parameter validation
- URL and CSV normalization
- Retry classification
- Identity evidence rules
- Match-decision behavior
- Repository helper logic

### Contract Tests

Contract tests verify stable behavior shared across implementations.

These tests cover areas such as:

- Adapter protocol compliance
- Required source-run fields
- Stable record identifiers
- Schema-version behavior
- Usage reporting
- Warning and failure structures
- Retryability semantics
- Immutability
- Idempotent reruns
- Consistent invalid-input handling

Reusable adapter behavioral contracts allow CSV, manual-record, and manual-URL implementations to be checked against the same expectations.

### Integration Tests

Integration tests verify interactions between application layers.

Representative scenarios include:

- Database migration lifecycle
- Repository round trips
- Configuration snapshot persistence
- Source-run persistence
- CSV import lifecycle
- Manual-record lifecycle
- Re-import behavior
- Idempotency repository behavior
- Identity candidate storage
- Merge lifecycle behavior
- Cross-repository consistency

### Acceptance Tests

Acceptance tests validate major stage outcomes rather than isolated functions.

They are used to confirm that a development stage:

- Implements its documented scope
- Preserves established behavior
- Keeps repository boundaries intact
- Does not alter unrelated schemas
- Satisfies zero-cost and safety constraints
- Passes the complete project suite
- Leaves the working tree in the expected state

A stage is not accepted merely because focused tests pass.

## Migration Testing

Database changes require dedicated lifecycle testing.

Migration validation includes:

- Fresh database creation
- Sequential migration application
- Current schema-version checks
- Repository compatibility
- Repeated initialization
- Full migration-chain assertions
- Detection of stale hard-coded migration expectations

When a new migration is added, the full tracked test tree must be checked for schema-version and migration-sequence assumptions.

## Adapter Testing

Source adapters are tested for both valid and invalid behavior.

### CSV Import

Representative checks include:

- Preview behavior
- Import behavior
- Locator precedence
- Source URL handling
- Website fallback
- Stable source-record identity
- Idempotent re-import
- Error counts
- Column alias validation
- Secret-like header rejection
- Tamper detection

### Manual Records

Representative checks include:

- Structured-record validation
- Stable identity
- Nested secret-like key rejection
- URL validation
- Lifecycle consistency
- Idempotent reruns
- Immutable input behavior

### Manual URLs

Representative checks include:

- URL normalization
- Credential-bearing URL rejection
- Stable locator behavior
- Invalid input handling
- Shared adapter-contract compliance

## Identity-Resolution Testing

Identity work is tested as a sequence of auditable stages rather than one opaque operation.

Coverage includes:

- Candidate generation
- Evidence storage
- Decision contracts
- Merge eligibility
- Stable source-record lineage
- Idempotent decisions
- Merge lifecycle behavior
- Conflict handling
- Preservation of canonical identity
- Rejection of invalid or ambiguous states

The system is designed to preserve evidence and explicit decisions instead of silently collapsing records.

## Configuration and Safety Testing

Configuration tests verify:

- Layered resolution
- Campaign-specific overrides
- Snapshot stability
- Secret-like key rejection
- Invalid capability combinations
- Zero-cost preflight behavior
- Usage-boundary enforcement
- Deterministic configuration manifests

Tests intentionally distinguish safe placeholders and field names from actual secret values.

## Zero-Cost Execution Testing

The zero-cost baseline is treated as an enforceable system boundary.

Tests cover:

- Source capability declarations
- Free-tier limits
- Blocked paid behavior
- Usage-ledger entries
- Preflight warnings
- Retry effects
- Configuration conflicts
- Campaign execution safeguards

The goal is to prevent a campaign from silently using a paid path when it was configured for zero-cost operation.

## Test Data Policy

The private test suite uses controlled fixtures.

The public showcase excludes:

- Real lead records
- Private organization data
- Personal contact details
- Client campaign information
- Credentials
- Proprietary matching fixtures
- Internal outreach examples

Any future public samples will use synthetic organizations, contacts, evidence, and campaign data.

## Deterministic Validation

Critical behavior is tested with deterministic assertions wherever practical.

This improves:

- Reproducibility
- Auditability
- Debugging
- Regression detection
- Stage acceptance
- Confidence in repeated imports

Semantic or AI-assisted features planned for later stages will remain behind explicit validation and human-review boundaries.

## Quality Gates

The private repository uses multiple quality checks, including:

- Pytest
- Ruff
- mypy
- Whitespace validation
- Focused stage tests
- Full-suite regression runs
- Migration-chain checks
- Repository cleanliness checks

Stage builders and completion scripts are expected to verify the actual repository tree before writing files.

## Current Evidence

The private repository contains hundreds of automated tests across:

- Unit tests
- Contract tests
- Integration tests
- Acceptance tests
- Migration lifecycle tests
- Adapter behavioral contracts
- Identity-resolution workflows
- Configuration and zero-cost safeguards

Exact counts change as the project evolves, so this public document emphasizes the structure and rigor of testing rather than presenting a fixed total.

## Private Test Boundary

The public repository intentionally excludes:

- Complete test source
- Internal fixtures
- Proprietary matching cases
- Full migration assertions
- Stage-builder scripts
- Private campaign datasets
- Acceptance patches
- Reconstructive implementation details

Selected test results and implementation details may be reviewed privately during a technical interview.
