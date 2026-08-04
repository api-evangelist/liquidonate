# LiquiDonate

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
