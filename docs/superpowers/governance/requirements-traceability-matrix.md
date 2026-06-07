# Requirements Traceability Matrix

This matrix is the authoritative bridge between the product design and phase plans. Detailed implementation plans may split requirements further but must not remove them.

| ID | Requirement | Owner Domain | Delivery Phase | Required Verification |
|---|---|---|---|---|
| REQ-001 | Workspace-scoped multi-tenancy | Identity & Tenancy | Phase 1 | RLS and cross-tenant denial tests |
| REQ-002 | Per-workspace memberships, roles, and permissions | Identity & Tenancy | Phase 1 | Role matrix integration tests |
| REQ-003 | Plans, subscriptions, entitlements, and limits | Subscriptions | Phase 1 | Entitlement allow/deny tests |
| REQ-004 | Organizer partner allocations and coupons | Subscriptions | Phase 1/3 | Quota, redemption, expiry, and idempotency tests |
| REQ-005 | Products, variants, SKU, barcode, and images | Seller Commerce | Phase 2 | CRUD, validation, and tenant isolation tests |
| REQ-006 | Inventory by variant and location | Inventory | Phase 2 | Movement and balance invariant tests |
| REQ-007 | Purchases, suppliers, expenses, and balances | Seller Commerce | Phase 2 | Ledger and supplier balance tests |
| REQ-008 | POS sales, shifts, returns, refunds, and split payments | Orders & POS | Phase 2 | Transactional workflow tests |
| REQ-009 | Offline POS with idempotent synchronization | Offline Sync | Phase 2 | retry, duplicate, and conflict tests |
| REQ-010 | Online/manual/social-channel order capture | Orders | Phase 2 | source and lifecycle tests |
| REQ-011 | Organizer profiles and bazaar lifecycle | Organizer Platform | Phase 3 | ownership and lifecycle transition tests |
| REQ-012 | Seller bazaar applications and exhibitor acceptance | Bazaar Hub | Phase 3 | application and acceptance tests |
| REQ-013 | Accepted bazaar creates seller sales event | Bazaar Hub | Phase 3 | cross-domain event contract test |
| REQ-014 | Public seller, organizer, and bazaar pages | Marketplace | Phase 4 | SSR, privacy, SEO, and moderation tests |
| REQ-015 | Reviewable marketplace product listings | Marketplace | Phase 4 | publish/reject/delist tests |
| REQ-016 | Structured public purchase request/order flow | Marketplace Orders | Phase 4 | public-to-seller order handoff tests |
| REQ-017 | Collections and featured placements | Promotions | Phase 5 | placement eligibility tests |
| REQ-018 | Promotion campaigns and spend records | Promotions | Phase 5 | budget and lifecycle tests |
| REQ-019 | Audit logs for privileged and financial operations | Platform Governance | Phase 1 onward | audit coverage tests |
| REQ-020 | Transactional domain event outbox | Platform Foundation | Phase 0/1 | commit/rollback and retry tests |
| REQ-021 | Arabic RTL and Egypt-first localization | Web Experience | All UI phases | mobile, desktop, RTL visual tests |
| REQ-022 | EGP-first money and Cairo business-time rules | Shared Kernel | Phase 0 | unit and serialization tests |
| REQ-023 | Public and private media with controlled access | Media | Phase 0/1 | Storage RLS and visibility tests |
| REQ-024 | Moderation queues for public content | Platform Admin | Phase 1/4 | approval and visibility tests |
| REQ-025 | Notifications and provider-neutral integrations | Notifications | Phase 0 onward | adapter contract and failure tests |
| REQ-026 | Local Supabase migrations, seed data, and generated types | Database Platform | Phase 0 | clean reset and type generation |
| REQ-027 | Backup, restore, observability, rate limiting, and abuse controls | Operations | Phase 0 onward | production readiness drills |

## Maintenance Rule

- New core requirements receive a new identifier before implementation.
- A requirement may be deferred only with a recorded product decision and target phase.
- Completion requires evidence in the relevant phase plan and release gate.

