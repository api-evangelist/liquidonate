---
name: Match a donation and attach your own shipping label
description: Use LiquiDonate MagicMatch to get a nonprofit match without buying LiquiDonate postage, then register the label you purchased yourself so the nonprofit is notified and the donation receipt is generated.
api: openapi/liquidonate-magicmatch-openapi.yml
operations:
  - match
  - attachLabelsToMatch
---

# Match a donation and attach your own shipping label

For integrators who already have carrier contracts and buy their own postage, but still want
LiquiDonate to pick the nonprofit and issue the donation receipt.

## The two-step contract

This is a **two-call flow and the second call is mandatory.** If you stop after `match`, the receiving
nonprofit is never notified of the incoming parcel and no donation receipt is generated — the
donation effectively does not exist in LiquiDonate's records.

## Step 1 — `match`

`POST /v1/match`

Send `donor`, `items[]` (each with `title`, `quantity`, `sku`, `categoryTypes[]`,
`conditionTagTypes[]`, `weightLbs`, optionally `price`) and `location.address`.

Returns `match.uuid`, the selected `match.nonprofit` with its full address, `match.distanceMiles` and
the normalized `match.items[]` with their assigned uuids.

Note what this operation does **not** do: it returns no shipping estimate and creates no shipment.
The `match.label` block comes back with empty strings and a `rate.value` of `"0"`. That is expected —
do not treat it as an error. If you want a rate, use `estimateMatchAndShip` instead (see
`skills/liquidonate-match-and-ship-return.md`).

Ship to the address on `match.nonprofit.address`.

## Step 2 — `attachLabelsToMatch`

Buy your postage with your own carrier, then immediately:

`POST /v1/match/ship`

```json
{
  "match_uuid": "<match.uuid from step 1>",
  "labels": [
    {
      "tracking_number": "<your tracking number>",
      "carrier": "USPS",
      "rate": { "amount": 10.20, "currencyCode": "USD" }
    }
  ]
}
```

Send one entry per label for multi-package shipments. The response echoes the updated match with
`labels[]` populated.

## Do not confuse this with `ship`

The `ship` operation buys a label between two fixed addresses. It performs **no nonprofit matching
and generates no donation receipt**, and it must not be paired with `match` — they are separate
workflows. Use `ship` only for warehouse-to-fixed-address movement where no donation is involved.

## Eligibility and errors

Same exclusions as the matching flow: no furniture, no mattresses, nothing over 50 kg, nothing with a
Damaged return reason — route those to `donate`.

`401 {"error":"invalid api key or secret"}` means one of the two required headers is missing or wrong.

## Idempotency

No `Idempotency-Key` support. `attachLabelsToMatch` is a state update against a known `match_uuid`,
so re-sending the identical label set is comparatively low risk, but LiquiDonate documents no dedupe
guarantee. Confirm the current state before retrying.
