---
name: Integrate ReturnsDirect order push and return webhooks
description: Wire a non-Shopify ecommerce platform into LiquiDonate ReturnsDirect - push order data with an HMAC-signed request, and verify and handle the seven outbound return and refund events.
api: openapi/liquidonate-returnsdirect-openapi.yml
operations:
  - pushExternalOrder
  - onReturnStatusChanged
  - onRefundStatusChanged
---

# Integrate ReturnsDirect order push and return webhooks

ReturnsDirect (Beta) is a two-way integration. You push orders in so LiquiDonate's customer-facing
return portal can resolve them; LiquiDonate pushes return and refund status events back so you can
update your own order management system.

## Environments

- Sandbox: `https://returns-sandbox.liquidonate.com`
- Production: `https://returns.liquidonate.com`

Build against sandbox first.

## Authentication — HMAC-SHA256, both directions

One shared secret per shop, provisioned out of band by LiquiDonate.

**Signing (you → LiquiDonate):**

```
signature = HMAC-SHA256(secret, rawRequestBody)
X-Signature:   <signature as lowercase hex>
X-Shop-Domain: <your shop identifier>
```

**Verifying (LiquiDonate → you):** LiquiDonate sends the same two headers. Recompute the digest and
compare with a **timing-safe** comparison (`crypto.timingSafeEqual` or equivalent) — never `==`.

Sign and verify over the **exact raw bytes**. Parsing JSON and re-serializing it before hashing is the
single most common cause of `401 {"error":"Invalid signature"}`; capture the raw body in your
framework's middleware before any body parser touches it.

Omitting either header returns `400 {"error":"Missing X-Shop-Domain or X-Signature header"}`
(verified live against both hosts).

## Step 1 — push orders with `pushExternalOrder`

`POST /webhooks/external-order` on every order create and update. LiquiDonate caches the order so the
return portal never has to call your API at return time.

`externalId` and `orderNumber` are required — omitting them returns
`400 {"error":"Missing required fields: externalId, orderNumber"}`.

Populate fully; the return portal's behaviour depends on it:

- customer: `customerName`, `customerEmail`, `customerPhone`
- shipping: `shippingAddress1`/`2`, `shippingCity`, `shippingState`, `shippingZip`,
  `shippingCountry`, `shippingLatitude`, `shippingLongitude`
- money: `originalShippingFee`, `shippingTaxAmount`
- `transactions[]` — `id`, `kind`, `status`, `amount`, `currency`, `paymentMethod`. Refunds are issued
  against `transactions[].id`, so this is not optional if you want refunds to work.
- `lineItems[]` — `externalLineItemId`, **`externalFulfillmentLineItemId`** (the id returns are
  processed against), `title`, `sku`, `quantity`, `pricePerUnit`, `discountAmountPerUnit`, `total`,
  `weight`, `weightUnit`, `categoryTypes[]`, `tags[]`
- `tags[]` on the order itself, for policy rules such as `final-sale`

A `200` returns `{"success": true, "id": <int>}`.

Orders are keyed on `externalId`, so re-pushing the same order updates the cached copy rather than
duplicating it. That is an upsert, not a documented idempotency guarantee — backfills are safe to
re-run, but do not assume exactly-once semantics.

## Step 2 — zero-config order lookup (optional)

For backlog orders that predate the integration, LiquiDonate can fetch on demand from your API. If
your order endpoint returns the **canonical ReturnsDirect camelCase shape** — the same field names as
the push body — LiquiDonate's built-in default mapper is used and no custom mapping code is written on
either side. A custom mapper is only needed when your field names or structure differ.

The retailer-side contract is `GET /orders/{id}`, `GET /orders?q=`, and `POST /returns/process`.

## Step 3 — handle the outbound events

Register your receiver URL with LiquiDonate. All seven events arrive as `POST` with the same payload
shape: `event`, `shop`, `parcelId`, `rmaId`, `externalOrderId`, `orderNumber`, `status`,
`refundStatus`, `refundAmount`, `refundMethod`, `updatedAt`.

Return lifecycle (`status`): `pending` → `pending_approval` → `approved` | `rejected` | `cancelled`

| Event | status | refundStatus |
|---|---|---|
| `return.pending` | pending | pending |
| `return.pending_approval` | pending_approval | pending |
| `return.approved` | approved | pending |
| `return.rejected` | rejected | pending |
| `return.cancelled` | cancelled | pending |
| `refund.completed` | approved | completed |
| `refund.flagged` | approved | flagged |

`refundAmount` is `null` until a refund resolves. `refund.flagged` means the refund was held for
review, **not** that money moved — do not mark the order refunded on that event.

Handler rules:

1. Verify the signature before parsing or trusting anything.
2. Respond `200` quickly; do the work asynchronously.
3. Events carry no delivery id and LiquiDonate documents no retry schedule, so **dedupe yourself** on
   `(rmaId, event, updatedAt)`.
4. Ignore out-of-order events by comparing `updatedAt` against your stored state rather than assuming
   ordered delivery.

The full event catalog is in `asyncapi/liquidonate-returnsdirect-asyncapi.yml`.

## Beta caveat

ReturnsDirect is labelled Beta by LiquiDonate and is unversioned — there is no version prefix, no
published deprecation policy and no Sunset header support. Pin nothing to a URL shape you cannot
change quickly.
