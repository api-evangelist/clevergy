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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Clevergy (The Clevergy Solution, SL) is a Madrid-based climate-tech SaaS company, founded in 2022,
selling white-label energy-management software to electricity and gas retailers, solar installers
and energy advisors. It ingests household consumption data — from Spanish distributors and the
Datadis smart-meter hub via the CUPS supply-point code, or from connected solar inverters and smart
devices — and turns it into analytics, invoice breakdowns, recommendations, appliance-level
disaggregation, virtual battery/wallet balances, energy-community shares and sales leads.

Two public integration surfaces are profiled here:

- **[Clevergy Connect API](https://docs.clever.gy/connect-api/clevergy-connect-api)** — a Swagger 2.0
  REST API on `connect.clever.gy`, 62 paths / 83 operations / 102 definitions, harvested verbatim to
  [`openapi/`](openapi/). Auth is a tenant API key in the `clevergy-api-key` header.
- **[Microfrontends](https://docs.clever.gy/helpdesk/microfrontends-catalog)** — ~30 embeddable web
  components catalogued in [`components/`](components/), loaded from a single unpinned ES module and
  authenticated with a 1-hour per-user JWT.

Sector: climate-tech. Source: portfolio company of
[serena](https://github.com/api-evangelist/serena) — https://clever.gy/en/
