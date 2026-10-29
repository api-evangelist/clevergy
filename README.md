# Clevergy

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

Clevergy (The Clevergy Solution, SL) is a Madrid-based climate-tech SaaS company, founded in 2022, that sells white-label energy-management software to electricity and gas retailers, solar installers and energy advisors. Its platform ingests household consumption data — pulled directly from Spanish distributors and the Datadis smart-meter hub via the CUPS supply-point code, or from connected solar inverters and smart devices — and turns it into consumption analytics, invoice breakdowns, savings recommendations, appliance-level disaggregation, virtual battery and wallet balances, energy-community shares, and sales leads the retailer can act on. Integrators consume it two ways: the Connect API, a Swagger 2.0 REST API on connect.clever.gy with 83 operations spanning users, houses, contracts, invoices, tariffs, equipment, energy telemetry, tickets and sales opportunities; and a catalog of roughly thirty embeddable "microfrontend" web components that drop Clevergy's end-user experiences straight into the retailer's own app or webview, authenticated with a short-lived per-user JWT. Clevergy raised a €3.2M round in October 2025 co-led by Racine² (Serena and Makesense) and Axon Partners Group, with Satgana, Wayra and Angels participating.

**Areas:** Company · Climate Tech · Energy · Energy Management · Utilities · Smart Meter · Solar · Home Energy · Battery Storage · EV Charging · Smart Home · Sustainability

## Clevergy on APIs.io

Everything profiled for Clevergy is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/clevergy/](https://apis.io/providers/clevergy/)** — or go straight to one of the APIs or properties below.

### APIs (25)

- [Clevergy Webhooks](https://apis.io/apis/clevergy/clevergy-webhooks/) — Outbound HTTP callbacks that notify an integrator in real time when a Clevergy lifecycle entity changes. Three event surfaces are documented — User,…
- [Clevergy Microfrontends](https://apis.io/apis/clevergy/clevergy-microfrontends/) — Roughly thirty embeddable UI components — custom elements (web components) rendering a React tree in an isolated container — that bring Clevergy's end-user…
- [Clevergy Access control API](https://apis.io/apis/clevergy/clevergy-access-control-api/) — The Access control API from Clevergy — 1 operation(s) for access control.
- [Clevergy Connections API](https://apis.io/apis/clevergy/clevergy-connections-api/) — The Connections API from Clevergy — 1 operation(s) for connections.
- [Clevergy Contracts API](https://apis.io/apis/clevergy/clevergy-contracts-api/) — The Contracts API from Clevergy — 1 operation(s) for contracts.
- [Clevergy Contracts - Deprecated API](https://apis.io/apis/clevergy/clevergy-contracts-deprecated-api/) — The Contracts - Deprecated API from Clevergy — 6 operation(s) for contracts - deprecated.
- [Clevergy Contracts|Electricity API](https://apis.io/apis/clevergy/clevergy-contracts-electricity-api/) — The Contracts|Electricity API from Clevergy — 2 operation(s) for contracts|electricity.
- [Clevergy Contracts|Gas API](https://apis.io/apis/clevergy/clevergy-contracts-gas-api/) — The Contracts|Gas API from Clevergy — 2 operation(s) for contracts|gas.
- [Clevergy Disaggregation API](https://apis.io/apis/clevergy/clevergy-disaggregation-api/) — The Disaggregation API from Clevergy — 1 operation(s) for disaggregation.
- [Clevergy Energy API](https://apis.io/apis/clevergy/clevergy-energy-api/) — The Energy API from Clevergy — 3 operation(s) for energy.
- [Clevergy Energy communities API](https://apis.io/apis/clevergy/clevergy-energy-communities-api/) — The Energy communities API from Clevergy — 3 operation(s) for energy communities.
- [Clevergy Equipments API](https://apis.io/apis/clevergy/clevergy-equipments-api/) — The Equipments API from Clevergy — 5 operation(s) for equipments.
- [Clevergy Houses API](https://apis.io/apis/clevergy/clevergy-houses-api/) — The Houses API from Clevergy — 6 operation(s) for houses.
- [Clevergy Integrations API](https://apis.io/apis/clevergy/clevergy-integrations-api/) — The Integrations API from Clevergy — 4 operation(s) for integrations.
- [Clevergy Invoices API](https://apis.io/apis/clevergy/clevergy-invoices-api/) — The Invoices API from Clevergy — 5 operation(s) for invoices.
- [Clevergy Invoices - Deprecated API](https://apis.io/apis/clevergy/clevergy-invoices-deprecated-api/) — The Invoices - Deprecated API from Clevergy — 2 operation(s) for invoices - deprecated.
- [Clevergy Power API](https://apis.io/apis/clevergy/clevergy-power-api/) — The Power API from Clevergy — 1 operation(s) for power.
- [Clevergy Sales Opportunities API](https://apis.io/apis/clevergy/clevergy-sales-opportunities-api/) — The Sales Opportunities API from Clevergy — 3 operation(s) for sales opportunities.
- [Clevergy Settings API](https://apis.io/apis/clevergy/clevergy-settings-api/) — The Settings API from Clevergy — 1 operation(s) for settings.
- [Clevergy Tariffs API](https://apis.io/apis/clevergy/clevergy-tariffs-api/) — The Tariffs API from Clevergy — 4 operation(s) for tariffs.
- [Clevergy Tickets API](https://apis.io/apis/clevergy/clevergy-tickets-api/) — The Tickets API from Clevergy — 3 operation(s) for tickets.
- [Clevergy Tips API](https://apis.io/apis/clevergy/clevergy-tips-api/) — The Tips API from Clevergy — 1 operation(s) for tips.
- [Clevergy Users API](https://apis.io/apis/clevergy/clevergy-users-api/) — The Users API from Clevergy — 6 operation(s) for users.
- [Clevergy Virtual battery API](https://apis.io/apis/clevergy/clevergy-virtual-battery-api/) — The Virtual battery API from Clevergy — 1 operation(s) for virtual battery.
- [Clevergy Virtual wallet API](https://apis.io/apis/clevergy/clevergy-virtual-wallet-api/) — The Virtual wallet API from Clevergy — 1 operation(s) for virtual wallet.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Clevergy Webhooks](https://apis.io/asyncapis/clevergy/clevergy-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [clevergy](https://apis.io/servers/clevergy/clevergy/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Clevergy Agentic Access](https://apis.io/agentic-access/clevergy/clevergy-agentic-access/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Clevergy Connect Api Overlay](https://apis.io/overlays/clevergy/clevergy-connect-api-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Clevergy Authentication](https://apis.io/security/clevergy/clevergy-authentication/)
- [Clevergy Domain Security](https://apis.io/security/clevergy/clevergy-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Clevergy Plans Pricing](https://apis.io/plans/clevergy/clevergy-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Clevergy Rate Limits](https://apis.io/rate-limits/clevergy/clevergy-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://clever.gy/en/)
- [Documentation](https://docs.clever.gy/)
- [Getting started](https://docs.clever.gy/developer/getting-started/authentication)
- [API reference](https://docs.clever.gy/connect-api/clevergy-connect-api)
- [Sign up](https://clever.gy/en/demo/)
- [Support](https://clever.gy/en/contacto/)
- [Blog](https://clever.gy/en/blog/)
- [Terms of service](https://clever.gy/en/terms-conditions-web/)
- [Privacy policy](https://clever.gy/en/politica-de-privacidad/)
- [GitHub](https://github.com/Clevergy)

## API Evangelist rating

**Kin Score 51.0** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 57.6 |
| Contract governance | 4.5 |
| Operational transparency | 10.5 |
| Developer ergonomics | 61.3 |
| Access clarity | 55.3 |

**Agent readiness 31.1** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
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
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 26.2** — Energy & Utilities

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/clevergy/](https://apis.io/providers/clevergy/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Clevergy: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/clevergy/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
