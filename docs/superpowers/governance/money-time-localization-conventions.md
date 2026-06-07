# Money, Time, and Localization Conventions

## Money

- Store amounts as integer minor units: `10050` represents `100.50 EGP`.
- Store an explicit ISO 4217 currency code with monetary documents.
- EGP is the default currency for the first market.
- Do not use JavaScript floating-point arithmetic for persisted financial totals.
- Centralize tax, discount, rounding, refund, and allocation calculations.
- Persist document totals and validate them against line calculations.

## Time

- Store event instants as `timestamptz` in UTC.
- Store business-only dates as `date`.
- Store bazaar timezone explicitly; default first-market timezone is `Africa/Cairo`.
- Subscription and coupon expiry semantics must state whether the boundary is an instant or end of local business day.
- Reports use the workspace reporting timezone, defaulting to `Africa/Cairo`.

## Localization

- Arabic RTL is the first-class interface.
- English-ready message keys are used from the start; visible product text is not hardcoded inside business logic.
- Egyptian phone numbers are normalized to an E.164-compatible form while retaining display formatting.
- Addresses use governorate, city/area, street/details, and optional location coordinates.
- Numbers, dates, and currency are formatted at the presentation boundary.

