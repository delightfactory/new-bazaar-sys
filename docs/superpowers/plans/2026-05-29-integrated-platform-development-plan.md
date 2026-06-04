# Integrated Platform Development Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create the master implementation control plan that keeps Bazaar Commerce Network & Marketplace development integrated, testable, secure, and aligned with the approved product design.

**Architecture:** The product will be implemented as a modular platform with clear domain boundaries, shared identity/tenancy foundations, entitlement-controlled access, event-driven side effects, and offline-first POS sync. Because the approved design spans multiple independent subsystems, this master plan governs execution order and quality gates, while each subsystem receives its own focused implementation plan before code is written.

**Tech Stack:** TypeScript-first web platform; Next.js App Router candidate for the web app; PostgreSQL as the relational source of truth; ORM/migration tool to be selected in the technical stack plan; PWA offline storage for POS; automated tests covering tenant isolation, permissions, entitlements, lifecycle transitions, inventory/ledger invariants, and sync idempotency.

---

## Planning Rule

The approved product design is too broad for one safe implementation plan. Development must be split into focused plans that each produce working, testable software without breaking platform integration.

No feature implementation may start until these items exist:

- A planning governance pack.
- A traceability matrix from design requirements to implementation plans and tests.
- A risk register with owners, mitigations, and review cadence.
- An architecture decision record log.
- A technical stack decision record.
- A domain map and dependency graph.
- A database foundation plan.
- A permission and entitlement matrix.
- A data classification and privacy map.
- A money, time, and localization convention record.
- An external integration adapter policy.
- A test strategy document.
- A rollout and verification checklist.

## Governance Operating Principle

Governance is a development accelerator, not a ceremony layer. Every required artifact must be compact, actionable, and tied to a real platform risk.

A governance control may block development only when it protects at least one of these areas:

- Tenant isolation.
- Permissions and entitlements.
- Money, payments, refunds, or ledger effects.
- Inventory correctness.
- Offline sync correctness.
- Public marketplace visibility and moderation.
- Customer or seller privacy.
- Irreversible architecture or provider decisions.
- Production reliability.

Governance controls must not block development for cosmetic preference, speculative scale, or documentation ceremony. When a safe default exists, use it and record the decision.

## Required Plan Documents

Create these implementation plans in this order:

1. `docs/superpowers/plans/2026-06-04-planning-governance-plan.md`
2. `docs/superpowers/plans/2026-05-29-phase-0-technical-foundation-plan.md`
3. `docs/superpowers/plans/2026-05-29-phase-1-identity-tenancy-entitlements-plan.md`
4. `docs/superpowers/plans/2026-05-29-phase-2-seller-commerce-pos-plan.md`
5. `docs/superpowers/plans/2026-05-29-phase-3-organizer-bazaar-hub-plan.md`
6. `docs/superpowers/plans/2026-05-29-phase-4-public-marketplace-plan.md`
7. `docs/superpowers/plans/2026-05-29-phase-5-promotions-growth-plan.md`
8. `docs/superpowers/plans/2026-05-29-production-readiness-plan.md`

Each plan must include:

- Files to create and modify.
- Database schema and migrations.
- API/domain services.
- UI routes and screens.
- Permission and entitlement checks.
- Tests with exact commands.
- Seed data or fixtures.
- Verification steps.
- Commit boundaries.

## Integration Contract

All subsystem plans must follow these contracts:

- Every private business entity includes `workspaceId`.
- Every user action is evaluated through workspace membership and role.
- Every paid or gated capability is checked through entitlements.
- Every public marketplace record is a separate publishable record with review status.
- Every stock-changing action creates inventory movements.
- Every money-changing action creates payment and ledger records.
- Every offline POS mutation has an idempotency key.
- Every important side effect emits a persisted domain event through an outbox.
- Every public form and coupon redemption path has rate limiting and abuse controls.
- Every admin action that changes access, money, public visibility, or stock is audited.
- Every money amount is stored as integer minor units with explicit currency code.
- Every business instant is stored in UTC; local business dates and event time zones are stored where business meaning depends on local time.
- Every external provider integration is hidden behind an internal adapter contract.
- Every uploaded asset has owner, visibility, size/type validation, and safe public-serving rules.
- Every public slug has uniqueness, reservation, and collision handling rules.
- Every import/export path enforces workspace ownership, permissions, validation, and auditability.

## Domain Dependency Order

Build domains in this dependency order:

1. Planning governance: traceability, risk register, ADR log, domain dependency map, conventions, and release gates.
2. Platform foundation: repository, stack, quality tooling, environment configuration.
3. Identity and tenancy: users, workspaces, memberships, roles, permissions.
4. Entitlements and plans: plans, subscriptions, entitlements, coupons, partner allocations.
5. Platform admin basics: admin roles, audit log browsing, manual moderation queues.
6. Seller commerce core: products, variants, locations, inventory movements.
7. Financial core: payment accounts, payments, payment allocations, ledger entries.
8. Orders and POS: sales, orders, returns, refunds, POS shifts.
9. Offline sync: device registration, local sale sync, idempotency, conflict states.
10. Organizer and Bazaar Hub: organizers, events, applications, exhibitors.
11. Marketplace publishing: public profiles, listings, review, public pages.
12. Marketplace order/request flow: public buyer, cart/request, order handoff to sellers.
13. Promotions: campaigns, placements, featured listings, reporting.
14. Production readiness: observability, backups, restore, rate limiting, security review.

## Cross-Cutting Quality Gates

Every phase must pass these gates before the next phase begins:

- `git status -sb` shows only intended changes before staging.
- Migrations apply on a fresh database.
- Tenant isolation tests pass.
- Permission tests pass for owner, manager, staff, cashier, organizer, platform admin, and public buyer paths relevant to the phase.
- Entitlement checks exist for every gated feature introduced in the phase.
- Audit logs exist for privileged mutations introduced in the phase.
- Domain events are persisted for critical side effects introduced in the phase.
- Error states are visible in UI for the phase's primary workflows.
- RTL and mobile layouts are verified for every new user-facing workflow.
- Public and seller-facing pages meet the phase's accessibility baseline.
- Money, date, and time displays follow the approved conventions.
- New external integrations use adapter boundaries and have failure-mode tests.
- New media upload or public content flows enforce file validation and visibility rules.
- New public routes have SEO, slug, and privacy checks where relevant.
- The phase has a small seed dataset for manual verification.
- The phase is committed separately with a clear message.

## Task 0: Create Planning Governance Plan

**Files:**
- Create: `docs/superpowers/plans/2026-06-04-planning-governance-plan.md`
- Reference: `docs/superpowers/specs/2026-05-29-bazaar-commerce-network-design.md`
- Reference: `docs/superpowers/audits/2026-06-04-plan-double-review.md`

- [ ] **Step 1: Write the planning governance plan**

Create `docs/superpowers/plans/2026-06-04-planning-governance-plan.md` with this exact header:

```markdown
# Planning Governance Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Establish traceability, risk management, architectural decision control, and cross-domain conventions before technical implementation begins.

**Architecture:** This plan creates the governance artifacts that every later implementation plan must reference. It prevents independent subsystem plans from drifting away from the approved product vision and integration contracts.

**Tech Stack:** Markdown governance artifacts in `docs/superpowers`, using requirement identifiers, ADR identifiers, risk identifiers, and checklist-driven verification.

---
```

The plan must create:

- `docs/superpowers/governance/requirements-traceability-matrix.md`
- `docs/superpowers/governance/risk-register.md`
- `docs/superpowers/governance/adr-log.md`
- `docs/superpowers/governance/domain-dependency-map.md`
- `docs/superpowers/governance/permission-entitlement-matrix.md`
- `docs/superpowers/governance/data-classification-privacy-map.md`
- `docs/superpowers/governance/money-time-localization-conventions.md`
- `docs/superpowers/governance/external-integration-adapter-policy.md`
- `docs/superpowers/governance/media-asset-policy.md`
- `docs/superpowers/governance/release-gates.md`

Each governance artifact should start as a concise working document. Expand it only when a later implementation plan needs more precision.

The plan must require these checks:

- Every requirement in the product design has an identifier.
- Every requirement maps to at least one phase plan or an explicitly deferred commercial decision.
- Every critical risk has mitigation and owner.
- Every high-impact architecture decision has an ADR entry.
- Every role/permission/entitlement combination is explicitly allowed or denied.
- Every PII field has owner, visibility, retention, and export rules.
- Money is represented as integer minor units with currency code.
- Time is stored as UTC instants plus local business date/time zone fields where needed.
- External integrations depend on internal adapter contracts.
- Media assets have ownership, validation, public visibility, and moderation rules.

- [ ] **Step 2: Verify the plan exists**

Run:

```powershell
Test-Path docs/superpowers/plans/2026-06-04-planning-governance-plan.md
```

Expected output:

```text
True
```

- [ ] **Step 3: Commit the planning governance plan**

Run:

```powershell
git add docs/superpowers/plans/2026-06-04-planning-governance-plan.md
git commit -m "docs: add planning governance plan"
```

Expected result: one commit containing only the planning governance plan.

## Task 1: Create Technical Foundation Decision Plan

**Files:**
- Create: `docs/superpowers/plans/2026-05-29-phase-0-technical-foundation-plan.md`
- Reference: `docs/superpowers/specs/2026-05-29-bazaar-commerce-network-design.md`

- [ ] **Step 1: Write the technical stack plan**

Create `docs/superpowers/plans/2026-05-29-phase-0-technical-foundation-plan.md` with this exact header:

```markdown
# Phase 0 Technical Foundation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Establish the project stack, repository structure, quality tooling, environment model, and architectural boundaries before feature implementation begins.

**Architecture:** Build a TypeScript-first modular web platform with a relational database, domain-oriented folder structure, shared testing utilities, and explicit configuration boundaries. This phase creates no business workflow beyond health checks and developer foundations.

**Tech Stack:** TypeScript, Next.js App Router candidate, PostgreSQL, ORM/migrations selected in this plan, Playwright for browser verification, unit/integration test runner selected in this plan, PWA support planned for POS phases.

---
```

Add sections for:

- Stack decision matrix.
- Repository folder structure.
- Domain boundary rules.
- Environment variable rules.
- ADR creation process.
- Traceability matrix update process.
- Risk register update process.
- Money/time/localization convention enforcement.
- External adapter folder and contract pattern.
- Media/asset handling foundation.
- Test tooling.
- Formatting and linting.
- Database migration workflow.
- Local development command.
- CI command set.
- Health check and environment validation.
- Structured logging baseline.
- Seed/reset workflow.
- Acceptance criteria.

- [ ] **Step 2: Verify the plan exists**

Run:

```powershell
Test-Path docs/superpowers/plans/2026-05-29-phase-0-technical-foundation-plan.md
```

Expected output:

```text
True
```

- [ ] **Step 3: Commit the technical foundation plan**

Run:

```powershell
git add docs/superpowers/plans/2026-05-29-phase-0-technical-foundation-plan.md
git commit -m "docs: add technical foundation plan"
```

Expected result: one commit containing only the technical foundation plan.

## Task 2: Create Identity, Tenancy, and Entitlements Plan

**Files:**
- Create: `docs/superpowers/plans/2026-05-29-phase-1-identity-tenancy-entitlements-plan.md`
- Reference: `docs/superpowers/specs/2026-05-29-bazaar-commerce-network-design.md`

- [ ] **Step 1: Write the Phase 1 plan**

The plan must cover:

- `User`
- `Workspace`
- `Membership`
- `Role`
- `Permission`
- `Plan`
- `Subscription`
- `Entitlement`
- `Coupon`
- `CouponRedemption`
- `PartnerAllocation`
- `AuditLog`
- `SystemEvent`

Required tests:

- A user cannot read another workspace's private data.
- A seller cannot access organizer-only actions without organizer entitlement.
- A cashier cannot change subscription or coupon settings.
- A coupon redemption is idempotent for repeated submissions.
- Revoked coupons cannot grant entitlements.
- Partner allocation usage cannot exceed quota.

- [ ] **Step 2: Verify the plan exists**

Run:

```powershell
Test-Path docs/superpowers/plans/2026-05-29-phase-1-identity-tenancy-entitlements-plan.md
```

Expected output:

```text
True
```

- [ ] **Step 3: Commit the Phase 1 plan**

Run:

```powershell
git add docs/superpowers/plans/2026-05-29-phase-1-identity-tenancy-entitlements-plan.md
git commit -m "docs: add identity tenancy entitlements plan"
```

Expected result: one commit containing only the Phase 1 plan.

## Task 3: Create Seller Commerce and POS Plan

**Files:**
- Create: `docs/superpowers/plans/2026-05-29-phase-2-seller-commerce-pos-plan.md`
- Reference: `docs/superpowers/specs/2026-05-29-bazaar-commerce-network-design.md`

- [ ] **Step 1: Write the Phase 2 plan**

The plan must cover:

- `Product`
- `ProductVariant`
- `InventoryLocation`
- `InventoryMovement`
- `InventoryTransfer`
- `SalesEvent`
- `Customer`
- `Supplier`
- `Purchase`
- `PurchaseItem`
- `Expense`
- `PaymentAccount`
- `LedgerEntry`
- `POSShift`
- `Device`
- `ImportJob`
- `Sale`
- `SaleItem`
- `Order`
- `OrderItem`
- `Payment`
- `PaymentAllocation`
- `Return`
- `Refund`
- `Fulfillment`
- `Shipment`

Required tests:

- Completed POS sale creates sale items, payment, inventory movement, ledger entry, and event.
- Purchase receipt increases stock and supplier balance.
- Return adjusts stock and ledger without deleting the original sale.
- Transfer creates paired outbound and inbound movements.
- POS shift closing calculates expected cash and difference.
- Offline sale sync is idempotent.
- Stock conflict is flagged when offline sale syncs against changed server stock.

- [ ] **Step 2: Verify the plan exists**

Run:

```powershell
Test-Path docs/superpowers/plans/2026-05-29-phase-2-seller-commerce-pos-plan.md
```

Expected output:

```text
True
```

- [ ] **Step 3: Commit the Phase 2 plan**

Run:

```powershell
git add docs/superpowers/plans/2026-05-29-phase-2-seller-commerce-pos-plan.md
git commit -m "docs: add seller commerce pos plan"
```

Expected result: one commit containing only the Phase 2 plan.

## Task 4: Create Organizer and Bazaar Hub Plan

**Files:**
- Create: `docs/superpowers/plans/2026-05-29-phase-3-organizer-bazaar-hub-plan.md`
- Reference: `docs/superpowers/specs/2026-05-29-bazaar-commerce-network-design.md`

- [ ] **Step 1: Write the Phase 3 plan**

The plan must cover:

- `OrganizerProfile`
- `BazaarEvent`
- `BazaarApplication`
- `BazaarExhibitor`
- `BazaarPage`
- `BazaarCategory`
- `BazaarSlot`
- Organizer onboarding.
- Bazaar application lifecycle.
- Accepted application creating seller-side `SalesEvent`.
- Dedicated inventory location creation when location-level stock tracking is enabled.
- Organizer coupon and invitation flows.

Required tests:

- Seller can apply to open bazaar.
- Seller cannot apply after applications are closed.
- Organizer can accept, reject, or waitlist applications for owned bazaar only.
- Accepted application creates exhibitor record and seller sales event.
- Public bazaar page cannot publish before platform approval.
- Organizer coupon campaign respects partner allocation quota.

- [ ] **Step 2: Verify the plan exists**

Run:

```powershell
Test-Path docs/superpowers/plans/2026-05-29-phase-3-organizer-bazaar-hub-plan.md
```

Expected output:

```text
True
```

- [ ] **Step 3: Commit the Phase 3 plan**

Run:

```powershell
git add docs/superpowers/plans/2026-05-29-phase-3-organizer-bazaar-hub-plan.md
git commit -m "docs: add organizer bazaar hub plan"
```

Expected result: one commit containing only the Phase 3 plan.

## Task 5: Create Public Marketplace Plan

**Files:**
- Create: `docs/superpowers/plans/2026-05-29-phase-4-public-marketplace-plan.md`
- Reference: `docs/superpowers/specs/2026-05-29-bazaar-commerce-network-design.md`

- [ ] **Step 1: Write the Phase 4 plan**

The plan must cover:

- `PublicSellerProfile`
- `MarketplaceListing`
- `ListingReview`
- `Collection`
- `PublicBuyer`
- `MarketplaceCart`
- Marketplace order source inside the shared order engine.
- Public seller pages.
- Public organizer pages.
- Public bazaar pages.
- Structured purchase request/order flow.
- Moderation workflow.

Required tests:

- Private product is not publicly visible.
- Submitted listing requires review before publishing.
- Rejected listing remains hidden.
- Published listing appears on seller page and marketplace search.
- Public buyer request creates real order record for the seller.
- Public buyer cannot access private customer, stock, ledger, or workspace records.

- [ ] **Step 2: Verify the plan exists**

Run:

```powershell
Test-Path docs/superpowers/plans/2026-05-29-phase-4-public-marketplace-plan.md
```

Expected output:

```text
True
```

- [ ] **Step 3: Commit the Phase 4 plan**

Run:

```powershell
git add docs/superpowers/plans/2026-05-29-phase-4-public-marketplace-plan.md
git commit -m "docs: add public marketplace plan"
```

Expected result: one commit containing only the Phase 4 plan.

## Task 6: Create Promotions and Growth Plan

**Files:**
- Create: `docs/superpowers/plans/2026-05-29-phase-5-promotions-growth-plan.md`
- Reference: `docs/superpowers/specs/2026-05-29-bazaar-commerce-network-design.md`

- [ ] **Step 1: Write the Phase 5 plan**

The plan must cover:

- `PromotionCampaign`
- `PromotedListing`
- `AdPlacement`
- `PromotionBudget`
- `PromotionSpend`
- Featured products.
- Featured sellers.
- Featured bazaar pages.
- Collection placement.
- Plan-based visibility advantages.

Required tests:

- Promotion cannot run for unpublished listing.
- Promotion cannot exceed entitlement or budget rules.
- Paused campaign stops featured placement.
- Rejected campaign never appears publicly.
- Promotion reporting uses tracked impressions and clicks when tracking is introduced.

- [ ] **Step 2: Verify the plan exists**

Run:

```powershell
Test-Path docs/superpowers/plans/2026-05-29-phase-5-promotions-growth-plan.md
```

Expected output:

```text
True
```

- [ ] **Step 3: Commit the Phase 5 plan**

Run:

```powershell
git add docs/superpowers/plans/2026-05-29-phase-5-promotions-growth-plan.md
git commit -m "docs: add promotions growth plan"
```

Expected result: one commit containing only the Phase 5 plan.

## Task 7: Create Production Readiness Plan

**Files:**
- Create: `docs/superpowers/plans/2026-05-29-production-readiness-plan.md`
- Reference: `docs/superpowers/specs/2026-05-29-bazaar-commerce-network-design.md`

- [ ] **Step 1: Write the production readiness plan**

The plan must cover:

- Environment separation.
- Secrets management.
- Backups.
- Restore drills.
- Observability.
- Error tracking.
- Rate limiting.
- Abuse controls.
- Marketplace moderation operations.
- Data retention.
- Admin access review.
- Security testing.
- Performance testing.
- Launch checklist.

Required verification:

- Backup can be restored to a clean environment.
- Tenant isolation tests run in CI.
- Rate limits exist for auth, public forms, and coupon redemption.
- Audit logs capture privileged mutations.
- Production launch cannot proceed with missing critical env vars.

- [ ] **Step 2: Verify the plan exists**

Run:

```powershell
Test-Path docs/superpowers/plans/2026-05-29-production-readiness-plan.md
```

Expected output:

```text
True
```

- [ ] **Step 3: Commit the production readiness plan**

Run:

```powershell
git add docs/superpowers/plans/2026-05-29-production-readiness-plan.md
git commit -m "docs: add production readiness plan"
```

Expected result: one commit containing only the production readiness plan.

## Final Verification for Planning Phase

- [ ] **Step 1: Confirm all required plan files exist**

Run:

```powershell
Test-Path docs/superpowers/plans/2026-06-04-planning-governance-plan.md
Test-Path docs/superpowers/plans/2026-05-29-phase-0-technical-foundation-plan.md
Test-Path docs/superpowers/plans/2026-05-29-phase-1-identity-tenancy-entitlements-plan.md
Test-Path docs/superpowers/plans/2026-05-29-phase-2-seller-commerce-pos-plan.md
Test-Path docs/superpowers/plans/2026-05-29-phase-3-organizer-bazaar-hub-plan.md
Test-Path docs/superpowers/plans/2026-05-29-phase-4-public-marketplace-plan.md
Test-Path docs/superpowers/plans/2026-05-29-phase-5-promotions-growth-plan.md
Test-Path docs/superpowers/plans/2026-05-29-production-readiness-plan.md
```

Expected output:

```text
True
True
True
True
True
True
True
True
```

- [ ] **Step 2: Search for unsafe planning markers**

Run:

```powershell
rg -n "T[B]D|T[O]DO|place[h]older|implement [l]ater|add [a]ppropriate|handle [e]dge cases|similar to [T]ask" docs/superpowers/plans
```

Expected result: no matches.

- [ ] **Step 3: Confirm git status**

Run:

```powershell
git status -sb
```

Expected result: branch is clean after all planning commits are pushed.
