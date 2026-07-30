# LiquiDonate

LiquiDonate is a San Francisco based reverse-logistics and donation-disposition platform that turns retail returns, excess inventory and unsellable goods into local nonprofit donations instead of landfill or liquidation. Its matching engine pairs items with nearby nonprofits that want them, buys the shipping label, routes the parcel and generates the donation receipt.

Backed by: uncork-capital

## APIs

| API | Docs | Base URL | Auth | Stage |
|---|---|---|---|---|
| MagicMatch by LiquiDonate | [docs-magicmatch.liquidonate.com](https://docs-magicmatch.liquidonate.com) | `https://api.liquidonate.com` | `X-LiquiDonate-Key` + `X-LiquiDonate-Secret` | GA (`/v1/`) |
| ReturnsDirect by LiquiDonate | [docs-returns.liquidonate.com](https://docs-returns.liquidonate.com) | `https://returns.liquidonate.com` | HMAC-SHA256 (`X-Signature` + `X-Shop-Domain`) | Beta |

Developer hub: [docs.liquidonate.com](https://docs.liquidonate.com). Both APIs are documented as public Postman collections; LiquiDonate publishes no OpenAPI or AsyncAPI of its own.

## Artifacts

- `openapi/` — OpenAPI 3.1 reconstructions of both APIs, built from the published Postman collections and verified against the live hosts
- `asyncapi/` — AsyncAPI 3.0 for the seven ReturnsDirect return/refund webhook events
- `overlays/` — API Evangelist enhancements over each spec
- `vocabulary/` — the 171-term `categoryTypes` item vocabulary MagicMatch matches on
- `skills/` — five Agent Skills, one per marquee flow
- `authentication/`, `conventions/`, `errors/`, `lifecycle/`, `conformance/`, `data-model/`, `sandbox/`, `packages/`, `well-known/`, `security/`, `mcp/`, `llms/`, `agentic-access/`

## Notes for integrators

- Neither API supports idempotency keys. `matchAndShip`, `ship` and `donate` purchase real postage — do not blind-retry.
- `estimateMatchAndShip` is the only reversible preview. Pass its `match_uuid` into `matchAndShip` to keep the same nonprofit and rate.
- After `match`, `attachLabelsToMatch` is mandatory when you buy your own label, or the nonprofit is never notified and no receipt is issued.
- Furniture, mattresses, items over 50 kg and Damaged-reason returns are excluded from matching — route them to `donate`.
- ReturnsDirect signatures must be computed over the exact raw request bytes.

Status: [status.liquidonate.com](https://status.liquidonate.com/) (Better Stack)
