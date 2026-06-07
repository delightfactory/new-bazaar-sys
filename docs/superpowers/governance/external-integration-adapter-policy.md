# External Integration Adapter Policy

Domain code depends on internal contracts, never directly on provider SDKs.

## Initial Ports

- `EmailSender`
- `WhatsAppLinkBuilder`
- `PaymentGateway`
- `ObjectStorage`
- `SearchIndex`
- `CaptchaVerifier`
- `AnalyticsSink`
- `ErrorReporter`

## Runtime Allocation

- Supabase Edge Functions: signed webhooks, short privileged endpoints, email dispatch, bounded integration calls.
- Supabase Queues: durable background messages and retryable work.
- Supabase Cron: schedules that enqueue work or invoke bounded functions.
- Node worker: jobs needing longer execution, richer libraries, or controlled concurrency.
- React Router Node server: SSR and BFF behavior; it must not become a second ungoverned database access layer.

## Failure Rules

- External calls have timeouts.
- Retry only idempotent operations.
- Provider identifiers are stored separately from domain identifiers.
- Webhook processing is idempotent and signature-verified.
- Provider outages do not roll back already committed core business transactions; compensating status is recorded.

