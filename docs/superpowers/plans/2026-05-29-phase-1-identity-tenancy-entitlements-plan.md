# Phase 1 Identity, Tenancy, and Entitlements Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Implement secure workspace isolation, memberships, role permissions, subscription entitlements, organizer partner allocations, coupons, audit logs, and the first platform-admin workflows.

**Architecture:** Supabase Auth identifies users; Postgres tables and RLS authorize access through workspace memberships. Roles and entitlements remain separate. Sensitive multi-table operations use transactional database functions in private schemas, while React Router server actions provide validated application entry points.

**Tech Stack:** Supabase Auth/Postgres/RLS, SQL migrations and pgTAP-style database tests, TypeScript generated database types, React Router actions/loaders, Zod validation, Vitest integration tests, Playwright workflow tests.

---

## Database Scope

Tables:

- `profiles`
- `workspaces`
- `workspace_memberships`
- `roles`
- `permissions`
- `role_permissions`
- `membership_roles`
- `plans`
- `plan_entitlements`
- `subscriptions`
- `workspace_entitlements`
- `coupons`
- `coupon_redemptions`
- `partner_allocations`
- `audit_logs`
- `domain_events`

Private functions:

- current workspace membership lookup,
- permission check,
- entitlement resolution,
- coupon redemption,
- partner allocation consumption,
- audit/event append.

### Task 1: Create Identity and Workspace Schema

**Files:**
- Create through `supabase migration new`: `identity_tenancy`
- Create: `supabase/tests/identity_tenancy.test.sql`
- Regenerate: `app/lib/database.types.ts`

- [ ] **Step 1: Generate migration**

```powershell
pnpm exec supabase migration new identity_tenancy
```

- [ ] **Step 2: Define tables and constraints**

Requirements:

- UUID primary keys.
- `profiles.id` references `auth.users(id)`.
- Workspace types: `seller`, `organizer`, `hybrid`, `platform`.
- Workspace statuses: `active`, `suspended`, `closed`.
- Membership statuses: `invited`, `active`, `suspended`, `removed`.
- Unique active membership per user/workspace.
- Indexed `workspace_id`, `user_id`, and status columns used by RLS.
- No role/permission data in user-editable metadata.

- [ ] **Step 3: Enable RLS**

Enable RLS on every exposed table and create explicit `TO authenticated` policies. Platform-wide tables that should not be directly queried remain in a private schema or have no client grants.

- [ ] **Step 4: Write database tests**

Test:

- user sees own active memberships,
- user cannot see another workspace,
- removed membership loses access,
- organizer and seller workspace types are preserved independently,
- update policies include corresponding select access.

- [ ] **Step 5: Verify**

```powershell
pnpm db:reset
pnpm db:test
pnpm db:types
```

Expected: pass and generated types update.

- [ ] **Step 6: Commit**

```powershell
git add supabase/migrations supabase/tests app/lib/database.types.ts
git commit -m "feat: add identity and workspace schema"
```

### Task 2: Implement Roles and Permissions

**Files:**
- Create through migration: role and permission schema/functions
- Create: `app/modules/identity/permissions.ts`
- Create: `app/modules/identity/authorization.server.ts`
- Test: `app/modules/identity/authorization.server.test.ts`
- Test: `supabase/tests/permissions.test.sql`

- [ ] **Step 1: Seed default permissions**

Use stable permission keys such as:

```text
workspace.manage
members.manage
catalog.read
catalog.write
inventory.read
inventory.adjust
sales.create
sales.refund
finance.read
suppliers.manage
marketplace.publish
bazaars.manage
applications.review
```

- [ ] **Step 2: Seed default roles**

Create owner, manager, staff, cashier, and organizer-manager role templates. Roles are copied/assigned per workspace without trusting client-supplied permission arrays.

- [ ] **Step 3: Implement authorization service**

`authorization.server.ts` must query current membership and permission truth from the database for sensitive actions. JWT application metadata may optimize non-sensitive UI hints but is not authoritative.

- [ ] **Step 4: Test**

Cover owner, manager, staff, cashier, organizer-manager, removed membership, and cross-workspace attempts.

- [ ] **Step 5: Verify and commit**

```powershell
pnpm db:reset
pnpm db:test
pnpm test -- app/modules/identity
git add supabase app/modules/identity app/lib/database.types.ts
git commit -m "feat: add workspace authorization"
```

### Task 3: Implement Workspace Onboarding

**Files:**
- Create: `app/modules/tenancy/schemas.ts`
- Create: `app/modules/tenancy/workspace.service.server.ts`
- Create: `app/routes/onboarding.tsx`
- Create: `app/routes/workspaces.$workspaceId.tsx`
- Test: `app/modules/tenancy/workspace.service.server.test.ts`
- Test: `tests/e2e/workspace-onboarding.spec.ts`

- [ ] **Step 1: Write failing service and workflow tests**

Test:

- new authenticated user creates one workspace,
- creator becomes owner atomically,
- duplicate retries do not create duplicate workspaces,
- seller and organizer profiles are initialized according to workspace type.

- [ ] **Step 2: Implement transactional creation**

Use a private transactional function invoked by the server action. The function creates workspace, membership, owner role assignment, audit log, and domain event in one transaction.

- [ ] **Step 3: Implement Arabic-first onboarding**

Collect business name, workspace type, category, default currency `EGP`, reporting timezone `Africa/Cairo`, and first inventory/payment defaults only when relevant.

- [ ] **Step 4: Verify**

```powershell
pnpm test -- app/modules/tenancy
pnpm test:e2e -- tests/e2e/workspace-onboarding.spec.ts
```

- [ ] **Step 5: Commit**

```powershell
git add app/modules/tenancy app/routes tests/e2e/workspace-onboarding.spec.ts supabase
git commit -m "feat: add workspace onboarding"
```

### Task 4: Implement Plans and Entitlements

**Files:**
- Create through migration: plans and entitlement tables/functions
- Create: `app/modules/subscriptions/entitlement-keys.ts`
- Create: `app/modules/subscriptions/entitlements.server.ts`
- Test: `app/modules/subscriptions/entitlements.server.test.ts`
- Test: `supabase/tests/entitlements.test.sql`

- [ ] **Step 1: Define entitlement model**

Support boolean, integer limit, and level/string values. Effective workspace entitlements resolve from:

1. active workspace override,
2. active subscription plan,
3. safe system default.

- [ ] **Step 2: Seed initial entitlement keys**

Use the keys in `permission-entitlement-matrix.md`.

- [ ] **Step 3: Implement server checks**

Provide:

```ts
requireEntitlement(workspaceId, key)
requireCapacity(workspaceId, key, currentUsage)
getEffectiveEntitlements(workspaceId)
```

- [ ] **Step 4: Test**

Cover active, grace period, suspended, override, expired, limit reached, and cross-workspace cases.

- [ ] **Step 5: Verify and commit**

```powershell
pnpm db:reset
pnpm db:test
pnpm test -- app/modules/subscriptions
git add supabase app/modules/subscriptions app/lib/database.types.ts
git commit -m "feat: add subscription entitlements"
```

### Task 5: Implement Coupons and Partner Allocations

**Files:**
- Create through migration: coupons, redemptions, partner allocations, private redemption function
- Create: `app/modules/subscriptions/coupon.schemas.ts`
- Create: `app/modules/subscriptions/coupons.server.ts`
- Create: `app/routes/redeem.$code.tsx`
- Test: `supabase/tests/coupon_redemption.test.sql`
- Test: `app/modules/subscriptions/coupons.server.test.ts`

- [ ] **Step 1: Define coupon rules**

Support:

- free duration,
- percentage discount,
- fixed minor-unit discount,
- specific plan grant,
- usage limit,
- one use per workspace,
- validity instant/local-day boundary,
- organizer/campaign/bazaar attribution.

- [ ] **Step 2: Implement atomic redemption**

The private function must lock coupon/allocation rows, validate all rules, create one redemption, update usage, grant subscription/entitlement effect, append audit/event records, and return the existing result for a repeated idempotency key.

- [ ] **Step 3: Test concurrency and abuse**

Test:

- exhausted coupon,
- expired coupon,
- revoked coupon,
- duplicate request,
- simultaneous final-slot redemption,
- partner allocation quota,
- unauthorized organizer.

- [ ] **Step 4: Add rate-limited route**

The public redemption route validates input and uses an abuse-control adapter. It never exposes internal allocation details.

- [ ] **Step 5: Verify and commit**

```powershell
pnpm db:reset
pnpm db:test
pnpm test -- app/modules/subscriptions
git add supabase app/modules/subscriptions app/routes app/lib/database.types.ts
git commit -m "feat: add partner coupon redemption"
```

### Task 6: Implement Audit Log and Domain Outbox

**Files:**
- Create through migration: append-only audit and domain event functions
- Create: `app/modules/platform/audit.server.ts`
- Create: `app/modules/platform/events.server.ts`
- Test: `supabase/tests/audit_outbox.test.sql`
- Test: `app/modules/platform/events.server.test.ts`

- [ ] **Step 1: Enforce append-only records**

Normal application roles cannot update/delete audit or event records.

- [ ] **Step 2: Store event fields**

Include:

- event ID,
- event type,
- aggregate type/ID,
- workspace ID when applicable,
- actor ID when applicable,
- payload,
- occurrence time,
- processing status and attempts.

- [ ] **Step 3: Test transaction behavior**

Verify:

- business transaction rollback also rolls back event,
- successful transaction persists event,
- repeated processing is idempotent,
- private payload is not publicly accessible.

- [ ] **Step 4: Verify and commit**

```powershell
pnpm db:reset
pnpm db:test
pnpm test -- app/modules/platform
git add supabase app/modules/platform app/lib/database.types.ts
git commit -m "feat: add audit and domain outbox"
```

### Task 7: Implement Platform Admin Foundation

**Files:**
- Create: `app/modules/platform/admin-authorization.server.ts`
- Create: `app/routes/admin.tsx`
- Create: `app/routes/admin.workspaces.tsx`
- Create: `app/routes/admin.audit.tsx`
- Create: `app/routes/admin.moderation.tsx`
- Test: `tests/e2e/platform-admin.spec.ts`

- [ ] **Step 1: Define platform roles**

Use least privilege:

- support,
- reviewer,
- billing operator,
- platform administrator.

- [ ] **Step 2: Implement server-only admin access**

Platform admin truth is stored in protected application data/app metadata controlled by the system, never user metadata.

- [ ] **Step 3: Add read-only foundation screens**

Start with workspace lookup, audit browsing, and empty moderation queue. Mutation tools are added only with explicit audit and permission tests.

- [ ] **Step 4: Verify and commit**

```powershell
pnpm test:e2e -- tests/e2e/platform-admin.spec.ts
git add app/modules/platform app/routes tests/e2e/platform-admin.spec.ts
git commit -m "feat: add platform admin foundation"
```

## Phase 1 Exit Criteria

- Cross-tenant read/write tests pass for all introduced tables.
- Role and entitlement checks are enforced server-side and in RLS/functions where applicable.
- Workspace creation is atomic and idempotent.
- Coupon redemption is atomic, quota-safe, rate-limited, and idempotent.
- Audit and domain events are append-only and transactional.
- Admin access follows least privilege.
- Generated database types match migrations.
- Arabic mobile onboarding passes Playwright verification.

