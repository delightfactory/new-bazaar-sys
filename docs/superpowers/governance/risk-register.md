# Risk Register

Only material risks are tracked. Review this file when a phase starts, when architecture changes, and before production release.

| ID | Risk | Severity | Mitigation | Owner | Review Point |
|---|---|---|---|---|---|
| RISK-001 | Cross-workspace data exposure | Critical | RLS on exposed tables, server checks, cross-tenant tests | Identity lead | Every schema phase |
| RISK-002 | Incorrect balances or stock drift | Critical | immutable movements/ledger, transactional functions, invariant tests | Commerce lead | Phase 2 and every financial change |
| RISK-003 | Duplicate offline sales | Critical | device IDs, idempotency keys, unique constraints, retry tests | POS lead | Phase 2 |
| RISK-004 | Stale or conflicting offline stock | High | server-authoritative sync, conflict records, visible resolution | POS lead | Phase 2 |
| RISK-005 | Coupon or entitlement abuse | High | atomic redemption, quotas, expiry, audit, rate limits | Subscription lead | Phase 1/3 |
| RISK-006 | Public content exposes private data | Critical | separate listing/public records, moderation, privacy tests | Marketplace lead | Phase 4 |
| RISK-007 | RLS performance degradation | High | indexed policy columns, simple policies, query-plan review | Database lead | Every large table |
| RISK-008 | Provider lock-in leaks into domains | Medium | internal adapter interfaces and provider-specific packages | Platform lead | Integration introduction |
| RISK-009 | Edge Functions used for long-running work | Medium | queues/workers for durable jobs, short idempotent functions | Platform lead | Phase 0 onward |
| RISK-010 | SEO failure from SPA-only marketplace | High | React Router SSR/pre-render for public routes | Web lead | Phase 0/4 |
| RISK-011 | Governance becomes a delivery bottleneck | Medium | risk-based gates, concise artifacts, safe defaults | Project lead | Monthly |
| RISK-012 | Supabase environment drift | High | migrations in Git, clean local reset, staging before production | Database lead | Every deployment |
| RISK-013 | Storage objects become publicly enumerable | High | private buckets by default, Storage RLS, signed/public delivery rules | Media lead | Phase 0/4 |
| RISK-014 | Public forms and marketplace spam | Medium | rate limiting, CAPTCHA adapter, abuse monitoring | Marketplace lead | Phase 4 |
| RISK-015 | Scope expansion blocks core delivery | High | dependency-based phases and release gates | Project lead | Every planning checkpoint |

