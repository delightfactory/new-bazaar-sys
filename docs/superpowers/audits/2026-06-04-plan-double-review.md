# Plan Double Review Audit

Date: 2026-06-04
Scope:

- `docs/superpowers/specs/2026-05-29-bazaar-commerce-network-design.md`
- `docs/superpowers/plans/2026-05-29-integrated-platform-development-plan.md`

## Executive Result

The product vision and master plan are directionally strong, but the plan needed stronger governance controls before implementation begins. The most important hidden risk was not a missing feature; it was the lack of mandatory traceability and decision-control artifacts that prevent subsystem plans from drifting away from the platform vision.

This audit recommends adding a planning governance layer before Phase 0 technical implementation. That layer must produce:

- Traceability matrix from design requirements to implementation plans and tests.
- Risk register with owners, mitigations, and review cadence.
- Architecture decision record log.
- Domain dependency map.
- Permission and entitlement matrix.
- Data classification and privacy map.
- Money, time, and localization conventions.
- Integration readiness checklist for external systems.

## Governance Balance Principle

The governance layer must prevent drift, not create an architecture cage.

Rules:

- Governance artifacts start compact and practical.
- A control is required only when it protects tenant isolation, money, permissions, public content, offline sync, irreversible architecture, or production reliability.
- A document should be short by default and expanded only when a real implementation decision needs it.
- No gate may exist only for ceremony.
- The team should prefer executable checks, tests, and clear ownership over long prose.
- Deferred commercial choices must be visible, but they should not block technical foundations when safe defaults exist.

## Critical Findings

### 1. No Traceability Matrix

Risk:
Without a traceability matrix, future implementation plans may accidentally skip important requirements such as offline sync idempotency, public content moderation, entitlement enforcement, or marketplace publishing states.

Required action:
Create a traceability matrix that maps each spec section to:

- Domain owner.
- Phase plan.
- Database entities.
- API/services.
- UI surfaces.
- Required tests.
- Release gate.

Status:
Added to the master plan as a required pre-implementation artifact.

### 2. No Formal Decision Control

Risk:
Major choices such as ORM, auth model, payment abstraction, file storage, search, PWA strategy, and deployment target could be made informally during development, creating rework or inconsistent implementation.

Required action:
Maintain ADRs for irreversible or expensive decisions.

Minimum ADR topics:

- Application framework.
- Database and ORM.
- Authentication/session strategy.
- Tenancy model.
- Permission model.
- Entitlement model.
- Offline sync architecture.
- Money representation.
- File/media storage.
- Search architecture.
- Deployment and CI.

Status:
Added to the master plan as required before feature work.

### 3. Money, Currency, and Financial Precision Need a Fixed Convention

Risk:
Using floating point or inconsistent amount formats can corrupt sales totals, discounts, refunds, commissions, and reports.

Required action:
All money amounts must be stored as integer minor units with explicit currency code. EGP is the first supported currency. Calculations must be centralized in money utilities and tested.

Status:
Added as an integration contract.

### 4. Time, Time Zone, and Bazaar Event Boundaries Need a Fixed Convention

Risk:
Bazaar dates, POS shifts, subscription expiry, coupon validity, and reports can break if local time and UTC are mixed casually.

Required action:
Store instants in UTC, store business dates and event local timezone separately where needed, and define display timezone behavior for Egypt-first launch.

Status:
Added as an integration contract.

### 5. External Integrations Were Named But Not Controlled

Risk:
WhatsApp, payment providers, email, storage, search, and analytics may leak provider-specific assumptions into core domains.

Required action:
All external systems must be behind provider-neutral adapters. Domain logic must depend on internal ports/contracts, not provider SDKs.

Status:
Added to the master plan.

### 6. Media and Public Content Pipeline Needs First-Class Treatment

Risk:
Product images, bazaar images, public pages, and receipts can create moderation, performance, storage, and security problems if treated as simple file uploads.

Required action:
Create an Asset/Media foundation with ownership, visibility, moderation status, image variants, size limits, and safe serving rules.

Status:
Added as a required Phase 0/Phase 1 planning concern.

### 7. UI/UX Quality Gates Were Too General

Risk:
Mobile-first and RTL can degrade during implementation if not tested continuously.

Required action:
Add required verification for:

- Mobile viewport.
- Desktop viewport.
- RTL layout.
- Key POS workflow.
- Public marketplace page.
- Organizer workflow.
- No overlapping text or broken responsive layout.

Status:
Added to quality gates.

### 8. Data Import/Export and Tenant Portability Need Early Design

Risk:
Sellers will need product import and possibly data export. If ignored early, import/export will bypass validations and cause inconsistent data.

Required action:
Model imports as jobs with validation reports and failure rows. Export must respect workspace ownership and permissions.

Status:
Added as governance and Phase 2 planning requirement.

### 9. Moderation Operations Need Workflow Ownership

Risk:
Marketplace quality depends on review queues, rejection reasons, appeals or edits, and admin accountability. Without operational workflow, public launch becomes unsafe.

Required action:
Define moderation queues, states, reasons, audit trail, and public visibility rules.

Status:
Already present conceptually; strengthened as a required readiness gate.

### 10. Production Readiness Should Start Earlier

Risk:
Backups, restore, observability, and rate limiting often arrive too late if left only for the final phase.

Required action:
Production readiness remains a final plan, but its minimum hooks must be introduced from Phase 0:

- Environment validation.
- Structured logging.
- Error boundaries.
- Health checks.
- Migration checks.
- Seed and reset workflows.

Status:
Added to master plan gates.

## Important Non-Blocking Findings

### 1. Direct Checkout Is Correctly Deferred

Current decision:
Start with structured purchase requests/orders, then add direct checkout.

Assessment:
This is safe as long as payment abstractions and order lifecycle exist from the beginning.

### 2. Microservices Are Correctly Deferred

Current decision:
Modular monolith with service-ready boundaries.

Assessment:
This remains the right architectural choice for stability, speed, and consistency.

### 3. Advanced Advertising Auction Is Correctly Deferred

Current decision:
Start with featured placements and campaign records.

Assessment:
Safe as long as promotion eligibility, placements, and reporting are modeled cleanly.

## Required Additions to the Master Plan

The master plan must now require:

- Planning governance pack before Phase 0.
- ADR log.
- Traceability matrix.
- Risk register.
- Domain dependency map.
- Money/time/localization conventions.
- Media/asset policy.
- External integration adapter policy.
- UI/RTL/mobile verification gate.
- Data import/export policy.
- Early production readiness hooks.
- A lightweight governance rule so controls stay useful and do not become a development bottleneck.

## Audit Conclusion

The project is ready to proceed into detailed planning only after a compact governance pack is created. The next safe step is not coding and not even Phase 0 implementation. The next safe step is to create the planning governance plan and then use it to produce the Phase 0 technical foundation plan. The governance pack should be practical and lean: enough to prevent drift, not enough to slow down healthy engineering momentum.

## Follow-Up Closure - 2026-06-07

The blocking recommendations from this audit are now addressed:

- The governance pack exists under `docs/superpowers/governance`.
- The technology baseline is fixed as React Router Framework Mode/Vite, TypeScript, Node.js 24 LTS, and Supabase.
- The Phase 0 technical foundation plan exists.
- The Phase 1 identity, tenancy, and entitlements plan exists.
- The master plan now uses rolling-wave planning, so later executable plans are completed before their phases without blocking Phase 0.

The project may proceed to Phase 0 implementation after the planning artifacts pass repository verification.
