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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

LiquiDonate is a San Francisco based reverse-logistics and donation-disposition platform that turns retail returns, excess inventory and unsellable goods into local nonprofit donations instead of landfill or liquidation. Its matching engine pairs items with nearby nonprofits that want them, buys the shipping label, routes the parcel and generates the donation receipt, so retailers recover tax value and meet extended-producer-responsibility and sustainability obligations. The company sells three product lines - ReturnsDirect (customer-direct returns), WarehouseDirect (warehouse and 3PL excess inventory) and BoxDrop (donation boxes) - and exposes two public developer APIs: MagicMatch, a donation-as-a-service API that accepts parcel and item details and returns a nonprofit match plus a donation shipping label, and ReturnsDirect, a beta returns-management integration for non-Shopify ecommerce platforms built on pushed order data plus HMAC-signed return and refund webhooks. LiquiDonate integrates with Shopify, Loop, EasyPost, FedEx, Bungii, SendGrid and Rise AI, and has been recognized on TIME Best Inventions 2025 and Fast Company World Changing Ideas.

**Areas:** Reverse Logistics · Returns Management · Donations · Non-Profit · Retail · E-Commerce · Sustainability · Circular Economy · Shipping · Supply Chain · Excess Inventory · Company

## LiquiDonate on APIs.io

Everything profiled for LiquiDonate is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/liquidonate/](https://apis.io/providers/liquidonate/)** — or go straight to one of the APIs or properties below.

### APIs (6)

- [LiquiDonate Donate API](https://apis.io/apis/liquidonate/liquidonate-donate-api/) — Bulky, multi-package and pickup-scheduled donations.
- [LiquiDonate Match API](https://apis.io/apis/liquidonate/liquidonate-match-api/) — Match items to nonprofits.
- [LiquiDonate Orders API](https://apis.io/apis/liquidonate/liquidonate-orders-api/) — Push order data into ReturnsDirect.
- [LiquiDonate Retailer API](https://apis.io/apis/liquidonate/liquidonate-retailer-api/) — Retailer account setup and lookup.
- [LiquiDonate Ship API](https://apis.io/apis/liquidonate/liquidonate-ship-api/) — Purchase donation shipping labels.
- [LiquiDonate ReturnsDirect By LiquiDonate (Beta) API](https://apis.io/apis/liquidonate/liquidonate-returnsdirect-by-liquidonate-beta-api/) — The ReturnsDirect By LiquiDonate (Beta) API from LiquiDonate — 0 operation(s) for returnsdirect by liquidonate (beta).

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [ReturnsDirect by LiquiDonate - Return and Refund Events](https://apis.io/asyncapis/liquidonate/liquidonate-returnsdirect-asyncapi/)

#### Event channels (1)

Individual events and webhooks the provider emits.

- [/webhooks/returnsdirect-rms](https://apis.io/channels/liquidonate/webhooks-returnsdirect-rms/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [liquidonate](https://apis.io/servers/liquidonate/liquidonate/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Liquidonate Agentic Access](https://apis.io/agentic-access/liquidonate/liquidonate-agentic-access/)

#### Collections (5)

Postman and Open Collection request collections.

- [MagicMatch by LiquiDonate Donate API](https://apis.io/collections/liquidonate/open-liquidonate-donate-api/)
- [MagicMatch by LiquiDonate Donate Match API](https://apis.io/collections/liquidonate/open-liquidonate-match-api/)
- [MagicMatch by LiquiDonate Donate Orders API](https://apis.io/collections/liquidonate/open-liquidonate-orders-api/)
- [MagicMatch by LiquiDonate Donate Retailer API](https://apis.io/collections/liquidonate/open-liquidonate-retailer-api/)
- [MagicMatch by LiquiDonate Donate Ship API](https://apis.io/collections/liquidonate/open-liquidonate-ship-api/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Liquidonate Magicmatch Overlay](https://apis.io/overlays/liquidonate/liquidonate-magicmatch-overlay/)
- [Liquidonate Returnsdirect Overlay](https://apis.io/overlays/liquidonate/liquidonate-returnsdirect-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Liquidonate Authentication](https://apis.io/security/liquidonate/liquidonate-authentication/)
- [Liquidonate Domain Security](https://apis.io/security/liquidonate/liquidonate-domain-security/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.liquidonate.com)
- [Documentation](https://docs.liquidonate.com)
- [Getting started](https://www.liquidonate.com/integrations)
- [API reference](https://docs.liquidonate.com)
- [Pricing](https://www.liquidonate.com/pricing)
- [Sign up](https://app.liquidonate.com/login)
- [Support](https://help.liquidonate.com)
- [Blog](https://www.liquidonate.com/blog)
- [Terms of service](https://www.liquidonate.com/terms-of-service)
- [Privacy policy](https://www.liquidonate.com/privacy-policy)
- [Status](https://status.liquidonate.com/)

## API Evangelist rating

**Kin Score 55.0** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 61.9 |
| Contract governance | 19.7 |
| Operational transparency | 23.7 |
| Developer ergonomics | 70.8 |
| Access clarity | 44.7 |

**Agent readiness 36.4** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | — |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 24.5** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/liquidonate/](https://apis.io/providers/liquidonate/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for LiquiDonate: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/liquidonate/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
