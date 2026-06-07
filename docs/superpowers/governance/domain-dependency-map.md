# Domain Dependency Map

## Dependency Direction

```text
Shared Kernel
  -> Identity & Tenancy
  -> Subscriptions & Entitlements
  -> Seller Catalog
  -> Inventory
  -> Finance
  -> Orders & POS
  -> Offline Sync
  -> Organizer Platform
  -> Bazaar Hub
  -> Marketplace Publishing
  -> Marketplace Orders
  -> Promotions
```

Platform Admin, Notifications, Analytics, Media, and Audit consume domain events and shared contracts. They must not become owners of business rules belonging to another domain.

## Ownership Rules

- Identity owns users, workspaces, memberships, and role assignments.
- Subscriptions owns plans, subscriptions, entitlements, coupons, and partner allocations.
- Catalog owns products and variants.
- Inventory owns stock movements, balances, transfers, and locations.
- Finance owns payment accounts, payments, allocations, and ledger entries.
- Orders owns order and sale lifecycles; it requests stock and finance effects through domain services.
- Offline Sync owns devices, sync batches, idempotency, and conflict records, not final sales truth.
- Organizer owns organizer profiles and bazaar definitions.
- Bazaar Hub owns applications and exhibitor relationships.
- Marketplace owns public profiles, listings, moderation state, and public discovery.
- Promotions owns campaigns and placements, not product publishing.

## Cross-Domain Rule

Domains communicate through typed application services and persisted events. Direct writes into another domain's tables are prohibited except inside explicitly reviewed database transactions/functions.

