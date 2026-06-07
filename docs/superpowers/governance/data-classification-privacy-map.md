# Data Classification and Privacy Map

| Class | Examples | Default Visibility | Controls |
|---|---|---|---|
| Public | approved listings, public seller name, public bazaar page | internet | moderation, slug rules, cache invalidation |
| Workspace Private | products before publishing, stock, suppliers, costs, reports | workspace members | RLS, role checks, audit for sensitive changes |
| Personal Data | customer phone, address, buyer identity, user profile | owning workspace and fulfillment actors | RLS, purpose limitation, export/delete workflow |
| Sensitive Operational | ledger entries, payments, coupon allocations, audit logs | restricted roles | server-only mutations, enhanced audit |
| Platform Confidential | secrets, service keys, provider credentials | server runtime only | secret manager, never client-exposed |

## Rules

- Public records are separate from private source records.
- Service-role credentials never enter browser bundles.
- Customer data collected for marketplace fulfillment is shared only with the fulfilling seller.
- Logs must avoid full addresses, access tokens, passwords, and secret values.
- Workspace export includes owned business data but excludes platform secrets and unrelated tenant records.
- Deletion behavior distinguishes user account deletion, workspace closure, financial retention, and public-content delisting.

