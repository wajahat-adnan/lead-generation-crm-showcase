# Privacy and Safety

## Overview

Lead Generation CRM is designed around local control, evidence-backed decisions, human approval, and strict separation between research data and outreach actions.

The production implementation is private. This document describes the intended privacy and safety boundaries without exposing private campaign data, proprietary matching logic, credentials, or internal operational procedures.

## Core Principles

- Keep campaign and lead data local by default
- Collect only information relevant to a defined campaign
- Preserve source provenance
- Never fabricate contact details or personalization
- Require human approval before outreach
- Keep credentials out of general configuration and imported metadata
- Respect configured source limits and zero-cost constraints
- Make uncertainty and failure visible
- Avoid publishing real lead or contact data in this showcase

## Local Data Handling

The application is designed to store operational data locally, including:

- Campaign configuration
- Imported source records
- Canonical organizations and contacts
- Evidence
- Identity candidates
- Match decisions
- Merge history
- Usage records
- Outreach authorization state

Local-first operation reduces unnecessary transfer of unpublished campaign research and contact data to external services.

Local storage does not remove the need for:

- Device security
- Operating-system access controls
- Encrypted backups where appropriate
- Careful export handling
- Secure credential storage
- Responsible retention and deletion policies

## Data Minimization

A source adapter should collect only fields needed for the campaign and supported by a legitimate source.

The system should avoid collecting unrelated personal information merely because it is technically accessible.

Campaign configuration is intended to define:

- Geographic scope
- Industry or niche
- Relevant organization attributes
- Required evidence
- Permitted sources
- Contact requirements
- Retention boundaries
- Outreach authorization rules

## Source Provenance

Imported data should remain traceable to its origin.

Source and evidence records are intended to preserve:

- Source type
- Source locator
- Source-run identity
- Collection context
- Relevant timestamps
- Adapter warnings
- Import failures
- Evidence supporting later decisions

Provenance allows a reviewer to distinguish verified source material from normalized, inferred, or manually supplied information.

## No-Fabrication Rule

The system must not invent:

- Email addresses
- Phone numbers
- Employee names
- Job titles
- Business claims
- Technology usage
- Trigger events
- Relationship history
- Personalized observations
- Evidence confidence

Missing information should remain missing, be marked as unknown, or be queued for human review.

AI-assisted features planned for later stages must remain downstream of structured evidence and explicit validation.

## Human Approval Boundary

No outreach should be sent merely because a lead was imported or scored.

Before sending, a human reviewer should be able to inspect:

- Organization identity
- Contact identity
- Supporting evidence
- Qualification reasons
- Contact-source provenance
- Personalization claims
- Draft message
- Campaign authorization
- Source and quota warnings

The reviewer must be able to approve, edit, defer, or reject the action.

## Identity Safety

Incorrectly merging two organizations or contacts can corrupt later research and outreach.

The identity architecture therefore separates:

- Potential candidates
- Supporting evidence
- Match decisions
- Merge execution
- Merge history

Ambiguous matches should remain unresolved until sufficient evidence or human review is available.

The system should preserve source lineage and avoid silently discarding conflicting records.

## Credential Safety

Credentials must not be stored in:

- Imported CSV fields
- Manual record metadata
- General configuration dictionaries
- Source URLs containing usernames or passwords
- Public samples
- Logs
- Repository history

The private implementation includes validation intended to reject secret-like keys and credential-bearing URLs from unsafe fields.

Actual credentials, when integrations are introduced, should use a dedicated secret-storage boundary rather than ordinary domain metadata.

## Repository Hygiene

The private source repository excludes local and sensitive artifacts through ignore rules, including:

- `.env` files
- Local databases
- Generated output
- Local lead data
- Virtual environments
- Test and analysis caches
- Logs
- Packaging artifacts

The public showcase excludes production source code and contains no operational lead database.

## Public Showcase Data Policy

Only synthetic data may be published in this repository.

Public examples must not contain:

- Real private lead lists
- Personal email addresses
- Personal phone numbers
- Client campaign data
- Authentication credentials
- Private notes
- Unpublished outreach history
- Proprietary source exports
- Data copied from restricted platforms
- Reconstructive matching or scoring fixtures

Synthetic samples should be clearly labeled and should not accidentally resemble a real private campaign.

## Contact Discovery

Planned contact discovery must distinguish between:

- Publicly presented business contact information
- Inferred contact patterns
- Unverified third-party data
- Missing contact information

An inferred address must never be represented as verified.

Where no reliable public contact method is available, the record should remain incomplete rather than being fabricated.

## Outreach Safety

The planned outreach workflow should include:

- Campaign-level authorization
- Per-message human approval
- Clear sender identity
- Accurate claims
- Evidence-backed personalization
- Controlled follow-up behavior
- Suppression and do-not-contact handling
- Outcome tracking
- Rate and quota awareness

The system is not intended to enable deceptive messaging, impersonation, or uncontrolled bulk spam.

## Platform and Source Boundaries

A technically accessible source is not automatically an approved source.

Adapters and campaign workflows should account for:

- Source terms
- Access restrictions
- Rate limits
- Public versus authenticated data
- Geographic requirements
- Campaign purpose
- Retention needs
- Evidence quality

The architecture separates source-specific collection from downstream identity and qualification decisions so each adapter can enforce its own boundaries.

## Zero-Cost Safety

The project has a strict zero-cost baseline.

The system is intended to:

- Declare source capabilities
- Track usage
- Warn as free limits are approached
- Block disallowed paid paths
- Avoid silent subscription or billing dependencies
- Make retry costs visible

Zero-cost configuration should be an enforceable execution constraint, not merely documentation.

## Logging and Diagnostics

Logs and diagnostic records should support troubleshooting without exposing unnecessary sensitive content.

Safer diagnostics emphasize:

- Record identifiers
- Stage names
- Error categories
- Counts
- Retryability
- Validation paths
- Source-run identifiers

Logs should avoid raw credentials, complete imported records, private message bodies, and unnecessary contact details.

## Retention and Deletion

Retention controls are planned as the application matures.

A complete workflow should eventually support:

- Campaign archival
- Record deletion
- Export review
- Backup lifecycle
- Suppression preservation
- Removal of stale evidence
- Deletion of generated outreach drafts
- Audit-aware cleanup

Deletion behavior must account for identity and merge history so removing data does not silently corrupt remaining records.

## Security Limitations

Local-first architecture does not by itself guarantee security.

Risk still depends on:

- Device access
- Malware protection
- Operating-system updates
- Backup handling
- Credential practices
- Export destinations
- User permissions
- Third-party integrations

The current project is an in-development engineering system, not a certified compliance product.

## Future Safety Work

Planned safety-related work includes:

- Dedicated credential storage
- Contact verification states
- Suppression lists
- Retention controls
- Export safeguards
- Outreach rate controls
- Source-specific policy metadata
- User-facing warnings
- Backup and recovery documentation
- Additional audit trails

## Private Implementation Boundary

This public document intentionally excludes:

- Real campaign data
- Contact records
- Credentials
- Complete validation rules
- Proprietary identity logic
- Internal threat models
- Operational source configurations
- Private outreach templates
- Security-sensitive implementation details
