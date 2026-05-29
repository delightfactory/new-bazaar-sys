# Bazaar Commerce Network & Marketplace - Product Design

Date: 2026-05-29
Status: Reviewed and strengthened for implementation planning
Target market: Egypt first, Arabic RTL first, expandable later

## 1. Vision

The product is not only an admin system for small sellers. It is a multi-sided commerce platform that connects sellers, bazaar organizers, and public buyers.

The platform combines:

- A mobile-first SaaS operating system for sellers.
- A professional portal for bazaar organizers.
- A private network where sellers can discover and apply to bazaar events.
- A public marketplace where approved products, sellers, organizers, and bazaar pages can be discovered by customers.

The long-term positioning is:

**Bazaar Commerce Network & Marketplace**

The product should feel simple on mobile, but be structurally strong enough to support subscriptions, partner coupons, organizer programs, public listings, promotions, and future commerce growth.

## 2. Product Principles

- Mobile-first, because most sellers operate from phones.
- Arabic RTL first, with Egypt as the first market.
- Professional depth without operational heaviness.
- One shared commerce core across online sales, bazaar POS, and marketplace orders.
- Offline-first POS for bazaar selling where internet may be weak or unavailable.
- Role-based experiences so sellers, organizers, platform admins, and public buyers each see the right interface.
- Platform-grade foundations from day one: tenancy, permissions, audit logs, entitlements, moderation, and event-driven domain boundaries.

## 2.1 Non-Negotiable Foundation Decisions

The following decisions are part of the foundation and must not be treated as optional implementation details:

- Every private business record belongs to a workspace.
- Every workspace has an explicit type: seller, organizer, platform, or hybrid.
- A user may belong to multiple workspaces, and their permissions are evaluated per workspace.
- Public marketplace data is published through reviewable public records, not by exposing private seller records directly.
- POS offline writes must be idempotent and synchronizable. The server remains the final source of truth after sync.
- Stock is tracked per product variant and per inventory location.
- Financial effects are recorded through payment and ledger entries, even when the UI presents them simply.
- Subscription access is controlled through entitlements, not only through subscription status.
- Public content requires moderation status before it appears to buyers.
- Domain events must be persisted through an outbox-style mechanism so critical side effects are not lost.

## 3. Primary User Types

### Seller

A small or growing business that sells through Instagram, Facebook, WhatsApp, bazaars, and later the public marketplace.

Sellers need:

- Product and inventory management.
- Fast mobile selling inside bazaars.
- Online order management.
- Customers, suppliers, purchases, expenses, and simple financial tracking.
- Bazaar discovery and application.
- Optional public marketplace visibility.

### Organizer / Partner

A bazaar organizer who can use the platform as an operational and growth tool.

Organizers need:

- Bazaar event creation and management.
- Seller applications.
- Exhibitor lists.
- Partner coupons and subscription offers.
- Public organizer and bazaar pages.
- Reports about event activity and seller engagement.

### Platform Admin

The internal team managing the platform, subscriptions, partners, content quality, marketplace publishing, and promotions.

### Public Buyer

A customer browsing the public marketplace, seller pages, organizer pages, and bazaar pages.

## 4. Core Product Layers

### 4.1 Seller Workspace

The seller workspace is the operating system for a seller's business.

Core capabilities:

- Products, variants, images, categories, cost, price, and stock.
- SKU and barcode fields, even if camera barcode scanning is introduced after the first seller workflow.
- Inventory locations such as main stock, branch, bazaar, or POS point.
- Inventory movements for sale, purchase, return, adjustment, and transfer.
- Moving average cost as the default cost method for simple profit reporting.
- Fast POS sales on mobile.
- Offline POS sale capture and later synchronization.
- Online orders from WhatsApp, Facebook, Instagram, manual entry, and marketplace.
- Customers, purchase history, notes, and balances.
- Suppliers, purchases, supplier balances, and payments.
- Expenses and payment accounts such as cash, bank, wallet, and Instapay.
- POS shifts/cash sessions for bazaar-day accountability.
- Daily and monthly reports.
- Product publishing controls for the public marketplace.

### 4.2 Organizer Portal

The organizer portal treats organizers as a first-class user type, not only as coupon distributors.

Core capabilities:

- Organizer profile.
- Bazaar creation with date, location, description, product categories, capacity, fees, terms, images, and application status.
- Bazaar application review: accept, reject, waitlist.
- Exhibitor management.
- Partner coupon and invitation distribution.
- Public organizer page.
- Public bazaar page.
- Reports for bazaar applications, exhibitors, and coupon usage.

### 4.3 Bazaar Hub

Bazaar Hub is the internal network that connects sellers with organizers.

Core capabilities:

- Sellers browse available bazaars.
- Filters by date, city, category, price, and application status.
- Sellers apply using their workspace profile and selected business information.
- Organizers review applications.
- When a seller is accepted, the platform creates a related sales event inside the seller workspace and may create a dedicated inventory location when the seller chooses location-level stock tracking for that bazaar.
- Bazaar Hub connects naturally with partner coupons and organizer campaigns.

### 4.4 Public Marketplace

The marketplace is a public layer under the platform brand. It is one central marketplace with many public hubs inside it.

Public hubs:

- Seller pages.
- Organizer pages.
- Bazaar event pages.
- Product listing pages.
- Collections and campaign pages.

The recommended model is:

**One Marketplace, Many Public Hubs**

This keeps marketplace trust, search, policies, moderation, and SEO centralized while giving sellers and organizers public value.

Core capabilities:

- Public product listings.
- Search and categories.
- Seller public profiles.
- Organizer public profiles.
- Bazaar public pages showing event details, exhibitors, and selected products.
- Marketplace orders that feed the same order engine used by sellers.
- Listing review and moderation before public publishing.
- Shipping, pickup, and customer-contact preferences as structured order fields, even if fulfillment starts manually.
- Egyptian phone, city, governorate, address, and EGP-first price formatting.

## 5. Subscription, Partner, and Coupon Model

The subscription system must be designed as a platform entitlement system, not only a payment flag.

Core concepts:

- `Plan`: commercial package and default limits.
- `Subscription`: active billing or access relationship for a workspace.
- `Entitlement`: actual granted access, limits, durations, and features.
- `Coupon`: redeemable code or link that grants discount, free time, or a specific plan.
- `CouponRedemption`: record of who redeemed what, when, and from which source.
- `PartnerAllocation`: quota or campaign allocation granted to an organizer or partner.

Entitlements must be able to control:

- Team member limits.
- Product and variant limits.
- Inventory location limits.
- Offline POS device/session limits.
- Marketplace publishing eligibility.
- Bazaar Hub access.
- Organizer portal access.
- Coupon allocation limits.
- Promotion and featured-placement eligibility.
- Reporting depth.

Organizer partner coupons can support:

- Free subscription for a specific duration.
- Discount percentage.
- Fixed discount.
- Specific plan access.
- Limited number of uses.
- Start and expiry dates.
- One use per seller or configured reuse rules.
- Bazaar-specific or campaign-specific linking.

The platform admin must be able to:

- Create organizers as partners.
- Allocate coupon quotas.
- Pause or revoke coupons.
- Monitor abuse.
- Track organizer performance as a growth channel.
- Later add commissions or partner revenue sharing.

## 6. Promotions and Advertising

Promotions should be a separate domain, not a simple boolean field on products.

Core concepts:

- `PromotionCampaign`: campaign owned by seller, organizer, or platform.
- `PromotedListing`: a product listing receiving paid or plan-based visibility.
- `AdPlacement`: where the promotion can appear.
- `PromotionBudget` and `PromotionSpend`: later support for budgeted ads.

Early use cases:

- Featured products.
- Featured sellers.
- Featured bazaar pages.
- Collection placement.
- Plan-based visibility advantages.

## 7. Architecture Direction

The recommended architecture is:

**Modular Monolith + Event-Driven Core + Service-Ready Boundaries**

This means the system is deployed initially as one platform application, but it is internally divided into clear domains with strong boundaries. This avoids premature distributed-system complexity while preserving a professional path to future service extraction.

Primary domains:

- Identity & Tenancy
- Seller Commerce
- POS & Offline Sync
- Orders Engine
- Organizer Platform
- Bazaar Hub
- Marketplace
- Subscriptions & Entitlements
- Promotions & Ads
- Platform Admin
- Notifications
- Analytics

Important technical patterns:

- Multi-tenant data model.
- Role-based access control.
- Domain events such as `SaleCompleted`, `CouponRedeemed`, `BazaarApplicationApproved`, and `ProductPublished`.
- Transactional outbox for domain events that trigger stock changes, notifications, entitlements, analytics, or marketplace publishing.
- Audit logs for sensitive operations.
- Feature flags and entitlements.
- Moderation workflows for public marketplace content.
- PWA offline strategy for POS.
- API boundaries between domains.

## 7.1 Offline POS and Sync Rules

Offline POS is a foundation requirement for bazaar selling. It must be designed as a controlled offline workflow, not as a general offline copy of the entire system.

Offline scope:

- Product catalog subset needed for selling.
- Variant, price, stock snapshot, and favorite products.
- Active inventory location or bazaar event.
- Active POS shift.
- Local sales, payments, discounts, and customer notes.

Sync rules:

- Each device has a stable device identifier.
- Each offline sale has a client-generated idempotency key.
- The server must accept repeated sync attempts without duplicating sales, payments, stock movements, or ledger entries.
- Synced sales create server-side sale records, inventory movements, payments, ledger entries, and analytics events.
- If stock changed while offline, the sale still syncs, but stock conflict is flagged for review rather than silently discarded.
- Negative stock may be allowed per workspace setting, but conflict visibility is mandatory.
- The user must see pending, synced, failed, and conflict states.

The server remains the final source of truth after synchronization.

## 7.2 Security, Privacy, and Operational Guardrails

Security and governance must be present from the first foundation phase:

- Tenant isolation must be enforced at every query boundary.
- Permissions must be checked server-side, not only in the UI.
- Public buyer access must never expose private seller, organizer, or customer records.
- Sensitive actions must create audit logs: role changes, subscription changes, coupon allocation, coupon redemption, marketplace approval, refund, stock adjustment, and admin impersonation if supported.
- PII such as phone numbers, addresses, and customer notes must be scoped to the owning workspace unless explicitly used in marketplace order fulfillment.
- Platform admins need least-privilege roles rather than one universal admin role.
- Backups, restore strategy, and production observability are required before production launch.
- Rate limiting and abuse controls are required for auth, coupon redemption, marketplace forms, and public application flows.

## 8. Data Model Overview

### Identity & Tenancy

- `User`
- `Workspace`
- `Membership`
- `Role`
- `Permission`

Users access business data through workspace membership. A workspace can represent a seller business, organizer entity, or internal platform context.

`Workspace` must include a workspace type and status. Supported workspace types are seller, organizer, platform, and hybrid. Hybrid workspaces may own both seller and organizer profiles, but each profile keeps its own domain data and permissions.

### Seller Commerce

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

### Sales & Orders

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

POS sales, online orders, and marketplace orders share common customer, item, payment, refund, and ledger concepts. POS may keep a dedicated `Sale` record for offline speed, but synced sales still create the same financial and inventory effects as orders.

### Organizer & Bazaar Network

- `OrganizerProfile`
- `BazaarEvent`
- `BazaarApplication`
- `BazaarExhibitor`
- `BazaarPage`
- `BazaarCategory`
- `BazaarSlot`

Accepted bazaar applications may create related seller-side sales events or inventory locations.

### Marketplace

- `PublicSellerProfile`
- `MarketplaceListing`
- `ListingReview`
- `Collection`
- `PublicBuyer`
- `MarketplaceCart`
- `MarketplaceOrder` as either a specialized order type or an order source.

Products do not appear publicly by default. Public visibility goes through listing status and review.

### Subscriptions & Entitlements

- `Plan`
- `Subscription`
- `Entitlement`
- `Coupon`
- `CouponRedemption`
- `PartnerAllocation`
- `BillingEvent`

### Promotions & Ads

- `PromotionCampaign`
- `PromotedListing`
- `AdPlacement`
- `PromotionBudget`
- `PromotionSpend`

### Platform Governance

- `AuditLog`
- `Notification`
- `ModerationCase`
- `SupportTicket`
- `SystemEvent`

## 8.1 Required Status Models and Lifecycles

The implementation plan must define explicit statuses before coding each domain. The default status models are:

### Product

- draft
- active
- archived
- blocked

### Marketplace Listing

- private
- submitted
- under_review
- approved
- rejected
- published
- paused
- delisted

Private seller products do not become marketplace listings until a seller explicitly submits them or enables publishing under allowed plan rules.

### Order

- draft
- placed
- confirmed
- preparing
- ready
- shipped
- delivered
- cancelled
- returned

Marketplace orders should start as structured purchase requests/orders. Direct payment checkout can be added later without changing the order lifecycle.

### Sale

- open
- completed
- synced
- sync_failed
- conflict
- refunded
- voided

### Bazaar Event

- draft
- submitted
- approved
- published
- open_for_applications
- applications_closed
- active
- completed
- cancelled

### Bazaar Application

- draft
- submitted
- under_review
- accepted
- waitlisted
- rejected
- withdrawn
- cancelled

### Coupon

- draft
- active
- paused
- expired
- exhausted
- revoked

### Subscription

- trialing
- active
- grace_period
- suspended
- cancelled
- expired

### Promotion Campaign

- draft
- scheduled
- active
- paused
- completed
- rejected

## 8.2 Financial and Inventory Invariants

These invariants prevent accounting and stock drift:

- Every completed POS sale creates sale items, payment records, inventory movements, and ledger entries.
- Every purchase that affects stock creates purchase items, inventory movements, supplier balance effects, and ledger entries.
- Refunds and returns must reverse or adjust stock, payments, and ledger effects through explicit records. They must not delete the original transaction.
- Stock adjustments require a reason and audit log.
- Transfers between locations create paired outbound and inbound stock movements.
- Customer and supplier balances are derived from orders, purchases, payments, allocations, refunds, and ledger entries.
- Payment records must support partial payment, split payment, overpayment handling, and allocation to customer or supplier balances.
- Profit reporting starts with moving average cost. More advanced cost methods are future extensions.
- POS shifts track opening cash, sales, refunds, expenses if allowed, expected cash, counted cash, and closing difference.

## 9. Main User Experiences

### Seller Mobile

The seller mobile home should prioritize daily action:

- Fast sale.
- New order.
- Products.
- Low stock.
- Today's sales.
- Available bazaars.
- Open orders.

Seller onboarding must include:

- Business name and category.
- Main sales channels.
- Default currency as EGP.
- First inventory location.
- First payment accounts.
- Optional sample products or spreadsheet import.
- Optional marketplace visibility setup.

The POS flow must support:

- Quick search.
- Favorite products.
- Quantity and discount.
- Payment method.
- Optional customer.
- Receipt sharing.
- Clear offline and sync status.
- Shift opening and closing when POS shifts are enabled.

### Seller Desktop

The desktop interface focuses on management:

- Dashboard.
- Products and inventory.
- Orders and sales.
- Customers and suppliers.
- Purchases and expenses.
- Reports.
- Marketplace publishing.
- Team and subscription settings.

### Organizer

The organizer interface focuses on events and seller relationships:

- My bazaars.
- Create bazaar.
- Applications.
- Exhibitors.
- Coupons and invitations.
- Organizer public page.
- Bazaar reports.

Organizer onboarding must include:

- Organizer public name and verification status.
- Contact information.
- Default city or operating area.
- Public page slug.
- Partner coupon eligibility if granted by platform admin.

### Public Marketplace

The public marketplace focuses on discovery and trust:

- Product browsing.
- Search and categories.
- Seller pages.
- Bazaar pages.
- Organizer pages.
- Featured products and collections.
- Structured purchase request/order flow.

The first marketplace commerce model is structured order/request flow, not fully automated direct checkout. It must still create real order records so direct checkout can be added later.

### Platform Admin

The admin interface controls platform quality and commercial operations:

- Users and workspaces.
- Marketplace review.
- Bazaar review.
- Subscriptions and entitlements.
- Coupons and partners.
- Promotions and featured placements.
- Moderation and support.
- Growth analytics.

## 10. Build Phases

### Phase 1: Foundation Platform

Build the platform roots:

- Authentication.
- Workspaces, memberships, and permissions.
- Seller and organizer workspace structures.
- Plans, subscriptions, entitlements, coupons, and partner allocations.
- Audit logs.
- File/media storage foundation for product images, public pages, receipts, and bazaar images.
- Notification foundation for in-app notifications, email-ready events, and WhatsApp share links.
- Domain event outbox.
- Basic platform admin.

### Phase 2: Seller Operating System

Build the seller's core business tools:

- Products and variants.
- Inventory and movements.
- Customers and suppliers.
- Purchases and expenses.
- POS sales.
- Offline sync.
- Orders and payments.
- Operational reports.
- Seller onboarding and product import.

### Phase 3: Organizer & Bazaar Hub

Build the network between organizers and sellers:

- Organizer portal.
- Bazaar creation.
- Internal bazaar publishing.
- Seller applications.
- Accept, reject, and waitlist flows.
- Link accepted bazaars to seller sales events.
- Organizer coupons and invitations.
- Organizer onboarding and public page setup.

### Phase 4: Public Marketplace

Build the public commerce layer:

- Seller public pages.
- Product listings and review.
- Organizer public pages.
- Bazaar public pages.
- Search and categories.
- Marketplace orders through the shared order engine.
- Public buyer inquiry/order flow.

### Phase 5: Promotions & Growth

Build monetization and growth tools:

- Featured products.
- Promotion campaigns.
- Collections.
- Paid or plan-based visibility.
- Promotion performance reports.
- Future organizer commissions or partner revenue sharing.

## 11. Out of Scope for the Initial Build Plan

These are important future capabilities but should not be in the first implementation plan unless explicitly prioritized later:

- Full microservices deployment.
- Complex accounting compliant with formal ERP standards.
- Advanced warehouse management.
- Native mobile apps.
- Full payment gateway settlement and split payments.
- AI-based recommendations.
- Public buyer reviews and dispute resolution.
- Advanced ad auction system.

The architecture should not block these features, but the first build plan should focus on the platform roots and the first operational workflows.

These items are not out of scope:

- Marketplace-ready product publishing states.
- Structured marketplace order/request records.
- Public seller, organizer, and bazaar pages.
- File/media storage foundations.
- Entitlement-based access checks.
- Offline POS sync foundations.
- Basic operational reporting.
- Moderation status for public content.

## 12. Deferred Business Decisions and Design Defaults

The following items do not block the product architecture. Each has a recommended default for the first implementation plan:

- Product name and brand language: defer until branding work starts.
- Payment gateway strategy for Egypt: design payment abstractions now; choose providers during implementation planning.
- Marketplace order model: start with structured purchase requests/orders, then add direct checkout when payment operations are ready.
- Organizer bazaar fees: record fee information in the event model, but keep payment collection outside the system at first unless explicitly prioritized.
- Marketplace search: begin with database-backed search and filters; design the marketplace domain so a dedicated search service can be added later.
- WhatsApp integration: start with share links and structured customer/order data; add deeper API integration after operational flows are stable.
- Subscription plan names, limits, and pricing: model plans and entitlements now; define commercial packaging separately before launch.

No deferred decision may remove a domain entity that the architecture depends on. Deferred commercial decisions may change configuration, pricing, providers, and UI emphasis, but not the existence of tenancy, entitlements, product publishing, bazaar applications, marketplace listings, orders, payments, inventory movements, ledger entries, or audit logs.

## 13. Development Readiness Checklist

Before implementation starts, the team must confirm that the first implementation plan includes:

- Database schema for all Phase 1 entities and any Phase 2 entities needed by foundational references.
- Explicit workspace isolation strategy.
- Permission matrix for seller, organizer, platform admin, staff, and cashier roles.
- Status enums for each lifecycle listed in this document.
- Event/outbox strategy.
- Offline POS sync contract.
- Basic media storage design.
- Entitlement checks for each gated feature.
- Moderation workflow for public listings, sellers, organizers, and bazaar pages.
- Test strategy covering tenant isolation, entitlement enforcement, coupon redemption, inventory movement, order status transitions, and offline sale sync idempotency.
