---
name: Donate a bulky, multi-package or pickup-scheduled item
description: Use LiquiDonate MagicMatch to submit oversized furniture, mattresses, multi-box products or items needing a scheduled pickup window, which fall outside the standard parcel match-and-ship flow.
api: openapi/liquidonate-magicmatch-openapi.yml
operations:
  - donate
---

# Donate a bulky, multi-package or pickup-scheduled item

The `donate` operation is the escape hatch for everything standard parcel shipping cannot carry.

## When to use this instead of `matchAndShip`

Route here when any of these is true:

- the item is large, oversized or awkward (furniture, mattresses, large appliances)
- it exceeds **50 kg**
- one product ships across **multiple boxes** and needs multiple labels
- it needs a **scheduled pickup window** rather than a drop-off
- it needs special handling or manual coordination with the receiving nonprofit

`matchAndShip` explicitly excludes furniture, mattresses and anything over 50 kg — those requests may
be rejected as ineligible. Send them here.

## Before you start

- Base URL `https://api.liquidonate.com`; both `X-LiquiDonate-Key` and `X-LiquiDonate-Secret` headers
  are required.
- `categoryTypes[]` values must come from `vocabulary/liquidonate-category-types.yml`.

## Call `donate`

`POST /v1/donate` with a single `item` object.

Required: `title` and `quantity`. Omitting the title returns
`400 {"code":"missing_item_title","error":"Item needs to have a title"}`.

Core fields to populate:

- `description`, `sizeType` (e.g. `medium`), `unitType` (e.g. `individual_item`)
- `categoryTypes[]`, `conditionTagTypes[]` (e.g. `new`)
- `fulfillmentTypes[]` — use `pickup` when collection is needed
- `recipientTypes[]` — e.g. `nonprofit`
- `fairMarketValue` as `{value, currencyCode}` — this drives the donation receipt value
- `imageURLs[]` — photos materially improve nonprofit acceptance for bulky goods
- `location` — the contactable pickup place, with `name`, `contactNumber` and a full `address`
- dimensions and weight: `weightLbs`, `lengthInches`, `heightInches`, `widthInches`

### Multi-package donations

Add `requestItemLabels[]` with one entry per box, each carrying `weightInLbs`. A desk shipping as
tabletop, legs and hardware is three entries — one label each, coordinated as one donation.

### Scheduled pickup

Add `pickupHours` with a `timezone` (IANA, e.g. `America/Los_Angeles`) and `pickupIntervals[]`, where
each interval has `days[]`, `startTimeLocal` and `endTimeLocal` in 24-hour `HH:MM`. Use this for
locations with restricted access hours such as offices, schools and warehouses.

## Response

`200` returns `item.uuid` — the handle for the submitted donation. There is no synchronous nonprofit
match or label here; the request enters LiquiDonate's coordination workflow.

## Idempotency

There is no `Idempotency-Key` header and no documented deduplication. Re-sending the same payload
after a timeout may create a second donation request. Store the returned `item.uuid` and confirm
before resubmitting.
