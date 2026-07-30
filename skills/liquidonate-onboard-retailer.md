---
name: Onboard a retailer and issue API credentials
description: Register a LiquiDonate retailer account (or a child retailer under an existing parent), capture the issued API key and secret, and verify the credentials resolve.
api: openapi/liquidonate-magicmatch-openapi.yml
operations:
  - setupRetailer
  - getRetailer
---

# Onboard a retailer and issue API credentials

The first call any MagicMatch integration makes. Everything else is authenticated with what this
returns.

## Step 1 — `setupRetailer`

`POST /v1/setupRetailer` at `https://api.liquidonate.com`

Two required objects:

- `retailer` — `name`, `businessPhoneNumber`, `businessAddress` (full `streetAddress1`, `city`,
  `state`, `zip`, `countryCode`), plus optional `website`, `description` and `shopifyShopID` when the
  retailer also runs the Shopify app.
- `user` — `firstName`, `lastName`, `email`, `phone` for the account owner.

The `200` response returns:

```json
{
  "retailer": { "uuid": "...", "name": "..." },
  "user":     { "uuid": "..." },
  "apiKey":   { "key": "ld_...", "secret": "ld_sk_..." }
}
```

**Capture `apiKey.key` and `apiKey.secret` immediately and store them in a secret manager.** They are
the only credentials for every other operation, and the documentation shows no endpoint to re-read or
rotate a secret. Never log them, never write them to a repo, never put them in a URL — they travel as
the `X-LiquiDonate-Key` and `X-LiquiDonate-Secret` headers.

## Parent and child retailers

LiquiDonate supports a parent/child retailer model. Call `setupRetailer` from the parent to create a
child (the docs' own example is named `LiquiDonate - Test Child`), then pass the child's
`retailer.uuid` as `retailer_uuid` on `matchAndShip` to act on its behalf.

If the uuid is not linked to the account that owns the API key you get
`403 {"code":"retailer_not_found","error":"child retailer not found"}`.

Because MagicMatch publishes no separate sandbox host, a child retailer is the documented way to
isolate a test integration from production traffic.

## Step 2 — verify with `getRetailer`

`POST /v1/getRetailer` with the new key and secret in the headers. It takes no body and returns the
retailer profile — `uuid`, `name`, `description`, `phone_number`, `website` — for whichever
credentials you sent.

This is the cheapest credential health check in the API and the only read-only operation in
MagicMatch. Use it as a connectivity smoke test before running any operation that spends money.

A `401 {"error":"invalid api key or secret"}` means the pair is wrong or one header is missing.
