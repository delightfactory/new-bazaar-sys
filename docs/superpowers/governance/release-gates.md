# Release Gates

Gates are evidence-based. A phase is blocked only by failure in a risk area relevant to that phase.

## Every Phase

- Intended migrations apply to a clean local Supabase database.
- Generated database TypeScript types are current.
- Relevant RLS and tenant isolation tests pass.
- Relevant permission and entitlement tests pass.
- Critical workflow tests pass.
- Audit and outbox behavior exists for introduced sensitive operations.
- Arabic RTL mobile and desktop screens are visually verified.
- No unresolved critical/high risk introduced by the phase.

## Public Feature Gate

- Moderation state prevents unapproved visibility.
- Private data cannot appear in public responses.
- SEO metadata, canonical URL, slug collision, and not-found behavior are tested.
- Public forms have abuse controls.

## Financial/Inventory Gate

- Atomic transaction tests pass.
- Duplicate/idempotency tests pass.
- Reversal behavior is tested without deleting source transactions.
- Reconciliation query or report exists.

## Production Gate

- Environment validation passes.
- Backup and restore drill is documented and successfully run.
- Observability and error reporting are active.
- Rate limits exist for auth, coupon redemption, webhooks, and public forms.
- Production migration has staging evidence and rollback/forward-fix notes.

