# Bazaar Commerce Network & Marketplace - Product Design

Date: 2026-05-29
Status: Ready for user review
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
- Inventory locations such as main stock, branch, bazaar, or POS point.
- Inventory movements for sale, purchase, return, adjustment, and transfer.
- Fast POS sales on mobile.
- Offline POS sale capture and later synchronization.
- Online orders from WhatsApp, Facebook, Instagram, manual entry, and marketplace.
- Customers, purchase history, notes, and balances.
- Suppliers, purchases, supplier balances, and payments.
- Expenses and payment accounts such as cash, bank, wallet, and Instapay.
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
- When a seller is accepted, the platform can create a related bazaar sales event or inventory location inside the seller workspace.
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

## 5. Subscription, Partner, and Coupon Model

The subscription system must be designed as a platform entitlement system, not only a payment flag.

Core concepts:

- `Plan`: commercial package and default limits.
- `Subscription`: active billing or access relationship for a workspace.
- `Entitlement`: actual granted access, limits, durations, and features.
- `Coupon`: redeemable code or link that grants discount, free time, or a specific plan.
- `CouponRedemption`: record of who redeemed what, when, and from which source.
- `PartnerAllocation`: quota or campaign allocation granted to an organizer or partner.

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
- Audit logs for sensitive operations.
- Feature flags and entitlements.
- Moderation workflows for public marketplace content.
- PWA offline strategy for POS.
- API boundaries between domains.

## 8. Data Model Overview

### Identity & Tenancy

- `User`
- `Workspace`
- `Membership`
- `Role`
- `Permission`

Users access business data through workspace membership. A workspace can represent a seller business, organizer entity, or internal platform context.

### Seller Commerce

- `Product`
- `ProductVariant`
- `InventoryLocation`
- `InventoryMovement`
- `Customer`
- `Supplier`
- `Purchase`
- `Expense`
- `PaymentAccount`
- `LedgerEntry`

### Sales & Orders

- `Sale`
- `SaleItem`
- `Order`
- `OrderItem`
- `Payment`
- `Return`
- `Refund`

POS sales, online orders, and marketplace orders should share common order/payment concepts where practical.

### Organizer & Bazaar Network

- `OrganizerProfile`
- `BazaarEvent`
- `BazaarApplication`
- `BazaarExhibitor`
- `BazaarPage`

Accepted bazaar applications may create related seller-side sales events or inventory locations.

### Marketplace

- `PublicSellerProfile`
- `MarketplaceListing`
- `ListingReview`
- `Collection`
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

The POS flow must support:

- Quick search.
- Favorite products.
- Quantity and discount.
- Payment method.
- Optional customer.
- Receipt sharing.
- Clear offline and sync status.

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

### Public Marketplace

The public marketplace focuses on discovery and trust:

- Product browsing.
- Search and categories.
- Seller pages.
- Bazaar pages.
- Organizer pages.
- Featured products and collections.
- Purchase or inquiry flow depending on the active commerce model.

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

### Phase 3: Organizer & Bazaar Hub

Build the network between organizers and sellers:

- Organizer portal.
- Bazaar creation.
- Internal bazaar publishing.
- Seller applications.
- Accept, reject, and waitlist flows.
- Link accepted bazaars to seller sales events.
- Organizer coupons and invitations.

### Phase 4: Public Marketplace

Build the public commerce layer:

- Seller public pages.
- Product listings and review.
- Organizer public pages.
- Bazaar public pages.
- Search and categories.
- Marketplace orders through the shared order engine.

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

## 12. Deferred Business Decisions and Design Defaults

The following items do not block the product architecture. Each has a recommended default for the first implementation plan:

- Product name and brand language: defer until branding work starts.
- Payment gateway strategy for Egypt: design payment abstractions now; choose providers during implementation planning.
- Marketplace order model: start with structured purchase requests/orders, then add direct checkout when payment operations are ready.
- Organizer bazaar fees: record fee information in the event model, but keep payment collection outside the system at first unless explicitly prioritized.
- Marketplace search: begin with database-backed search and filters; design the marketplace domain so a dedicated search service can be added later.
- WhatsApp integration: start with share links and structured customer/order data; add deeper API integration after operational flows are stable.
- Subscription plan names, limits, and pricing: model plans and entitlements now; define commercial packaging separately before launch.
