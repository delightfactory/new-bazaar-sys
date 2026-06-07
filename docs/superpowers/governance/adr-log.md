# Architecture Decision Log

Only decisions that are expensive to reverse require full ADRs. The status values are `accepted`, `superseded`, or `proposed`.

| ADR | Decision | Status | Rationale |
|---|---|---|---|
| ADR-001 | Modular monolith with explicit domain boundaries | accepted | Preserves consistency without distributed-system overhead |
| ADR-002 | Supabase as managed backend platform | accepted | Provides Postgres, Auth, Storage, Realtime, Queues, Cron, and local tooling |
| ADR-003 | React Router Framework Mode with Vite | accepted | Supports SSR/pre-render for public pages and client-heavy authenticated workflows |
| ADR-004 | TypeScript across web, Node, and Edge Functions | accepted | Shared types and existing team expertise |
| ADR-005 | SQL-first Supabase migrations; no ORM initially | accepted | Keeps RLS, functions, constraints, and migrations explicit |
| ADR-006 | Node runtime for SSR/BFF and durable workers | accepted | Edge Functions remain short-lived and integration-focused |
| ADR-007 | Supabase Edge Functions for webhooks and bounded privileged operations | accepted | Good fit for globally distributed short TypeScript handlers |
| ADR-008 | Supabase Queues for durable asynchronous jobs | accepted | Postgres-native delivery and operational simplicity |
| ADR-009 | IndexedDB with Dexie for offline POS state | accepted | Mature local transaction/query model and explicit sync queue |
| ADR-010 | Integer minor units plus ISO currency code for money | accepted | Prevents floating-point financial errors |
| ADR-011 | UTC instants plus explicit business date/timezone where required | accepted | Preserves event and subscription semantics |
| ADR-012 | pnpm workspace without Turborepo initially | accepted | Shared packages without unnecessary build orchestration |
| ADR-013 | React Router data APIs as default server-data mechanism | accepted | Avoids duplicate data frameworks; TanStack Query requires a demonstrated need |
| ADR-014 | Tailwind CSS, shadcn/ui patterns, and Lucide icons | accepted | Accessible, maintainable UI foundation compatible with RTL customization |
| ADR-015 | Vitest, Testing Library, Playwright, and SQL/RLS tests | accepted | Covers units, components, workflows, database rules, and visual behavior |
| ADR-016 | Rolling-wave detailed planning | accepted | Governance and dependency contracts are fixed now; each later phase receives an executable plan before implementation without blocking earlier phases |
| ADR-017 | No pg_graphql dependency | accepted | The application uses supabase-js/PostgREST, server actions, and explicit SQL functions; GraphQL defaults and introspection changes do not affect the platform |

## ADR Creation Threshold

Create a standalone ADR only when changing:

- framework or runtime,
- database or tenancy model,
- authorization or entitlement model,
- offline synchronization,
- financial representation,
- deployment topology,
- public rendering strategy,
- or a provider choice that changes domain contracts.
