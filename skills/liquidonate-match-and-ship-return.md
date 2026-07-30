---
name: Match a return to a nonprofit and ship it
description: Use LiquiDonate MagicMatch to preview a nonprofit match and shipping rate for a customer return, then commit the shipment and buy the donation label in a way that keeps the previewed match and price.
api: openapi/liquidonate-magicmatch-openapi.yml
operations:
  - estimateMatchAndShip
  - matchAndShip
---

# Match a return to a nonprofit and ship it

Turns a customer-direct return into a donation: LiquiDonate picks a nearby nonprofit that wants the
items, buys postage, and generates the donation receipt.

## Before you start

- Base URL is `https://api.liquidonate.com`.
- Every request needs **both** headers: `X-LiquiDonate-Key` (prefix `ld_`) and
  `X-LiquiDonate-Secret` (prefix `ld_sk_`). Missing or wrong values return
  `401 {"error":"invalid api key or secret"}`.
- Check eligibility first. Do **not** send these to this flow — they are excluded:
  - categories `furniture`, `mattress`, drug paraphernalia
  - anything over **50 kg**
  - anything whose return reason is **Damaged**

  Route those to the `donate` operation instead (see
  `skills/liquidonate-donate-bulky-item.md`).
- Item categories must come from the published vocabulary in
  `vocabulary/liquidonate-category-types.yml` (171 terms). Sending a term outside that list is not
  supported.

## Step 1 — preview with `estimateMatchAndShip`

`POST /v1/matchAndShip/estimate`

Send `donor`, `location.address`, `parcel.weight` (pounds) and an `items[]` array where each item has
`title`, `quantity`, `categoryTypes[]` and `weightLbs`.

This creates **no shipment and buys no postage**. It returns `match.uuid`, the chosen
`match.nonprofit`, `match.distanceMiles` and the estimated rate on `match.label.rate.value`.

Surface the nonprofit name and the rate to the user before going further. This is the only reversible
step in the flow.

## Step 2 — commit with `matchAndShip`

`POST /v1/matchAndShip`

Send the same body **plus `match_uuid`** set to the `match.uuid` you got back in step 1. Passing it
through is what keeps the same nonprofit and the same shipping rate; omit it and you may get a
different match at a different price.

Optional fields worth setting: `order_id` and `rma_id` to tie the donation back to your own records,
and `retailer_uuid` when acting on behalf of a linked child retailer.

The response carries `match.label.trackingNumber`, `match.label.carrier` and
`match.label.labelURL` — the downloadable postage label to hand to the customer.

## Error handling

| Status | Code | What to do |
|---|---|---|
| 401 | — | Both key and secret headers must be present and valid. |
| 400 | `retrieve_rate_error` | Rate lookup failed. Check the addresses resolve and the parcel weight is within carrier limits. |
| 403 | `retailer_not_found` | The `retailer_uuid` is not a child of the account that owns the API key. |

## Critical: this operation is not idempotent

LiquiDonate publishes **no** `Idempotency-Key` header and no documented replay guard. `matchAndShip`
purchases real postage. Never blind-retry it on a timeout or an ambiguous failure — you may buy a
second label and be billed twice.

On an uncertain outcome, do not retry automatically. Escalate to a human, and reconcile against the
`trackingNumber` you already hold before issuing any new call.

See `conventions/liquidonate-conventions.yml` and `errors/liquidonate-problem-types.yml`.
