---
name: get-stay-quote
description: >-
  Calculate the exact cost of a stay at Edelweiss Hotel Polyana for given dates,
  room type, and guests, then hand the guest a direct booking URL.
api: Edelweiss Hotel Polyana API
method: generated
generated: '2026-09-18'
source: openapi/edelweiss-hotel-polyana-api-openapi.json (operationId calculateStayQuote)
operations:
  - calculateStayQuote
---

# Get a stay quote

Use the open, no-auth Edelweiss Hotel Polyana API to price a stay.

## Steps

1. Collect `check_in` and `check_out` as `YYYY-MM-DD` (both required).
2. Pick a `room` slug — one of `standart`, `superior-triple`, `classic-lux`, `lux`
   (defaults to `standart`). Set `guests` (default 2). Optionally set
   `meals=true` for full board (600 UAH/person/day) and `military=true` for the
   10% Armed Forces of Ukraine discount.
3. Call `GET https://edelweiss-hotel.com.ua/api/quote/` with those query params.
   Note the trailing slash — the no-slash form 308-redirects to it, so follow
   redirects. No authentication.
4. Read `quote.costBreakdown` for the itemized total in UAH
   (baseAccommodationUah, militaryDiscountUah, netAccommodationUah, mealsUah,
   extraBedUah, resortTaxUah, grandTotalUah).
5. Present the total and give the guest `quote.directBookingUrl` to book
   directly (best-rate, zero commission).

## Conventions and gotchas

- Resort tax is 43 UAH/day/person (exempt: children under 18, veterans,
  disabled persons) — it is already in the breakdown.
- The endpoint is read-only and idempotent; there is no booking commit here, so
  nothing to reverse. Booking happens off-API via `directBookingUrl`.
- Response is `application/json` with wide-open CORS.
