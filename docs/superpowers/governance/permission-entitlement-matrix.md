# Permission and Entitlement Matrix

Roles answer **who may act**. Entitlements answer **whether the workspace has the feature or capacity**. Both checks are required.

## Default Roles

| Capability | Owner | Manager | Staff | Cashier | Organizer Manager | Platform Reviewer | Platform Admin |
|---|---:|---:|---:|---:|---:|---:|---:|
| Manage workspace and members | yes | limited | no | no | limited | no | platform only |
| Manage products and inventory | yes | yes | yes | read/sell | no | no | support only |
| Complete POS sales | yes | yes | yes | yes | no | no | no |
| Refund/void sale | yes | yes | policy-based | no | no | no | support only |
| View cost/profit | yes | yes | policy-based | no | no | no | support only |
| Manage suppliers/purchases | yes | yes | policy-based | no | no | no | no |
| Publish marketplace listing | yes | yes | policy-based | no | no | review | override |
| Create/manage bazaar | if organizer | if organizer | no | no | yes | review | override |
| Review bazaar applications | if organizer | if organizer | no | no | yes | no | override |
| Allocate partner coupons | no | no | no | no | entitlement-limited | no | yes |
| Review public content | no | no | no | no | no | yes | yes |
| Change plans/entitlements | no | no | no | no | no | no | yes |

## Entitlement Families

- `team.members.max`
- `catalog.products.max`
- `catalog.variants.max`
- `inventory.locations.max`
- `pos.offline.enabled`
- `pos.devices.max`
- `marketplace.publish.enabled`
- `bazaar_hub.access`
- `organizer.portal.enabled`
- `partner.coupons.max_active`
- `reports.level`
- `promotions.enabled`

## Enforcement Rule

- UI checks improve usability but are never authoritative.
- Server actions and database policies/functions enforce access.
- Authorization data must not rely on user-editable metadata.
- Sensitive membership changes require immediate server checks; JWT claims are not the sole source of truth.

