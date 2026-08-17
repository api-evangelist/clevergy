---
name: Sync Clevergy sales opportunities into a CRM
description: >-
  Receive Clevergy's sales-opportunity webhooks, fetch the full lead from the Connect API, and keep
  its status in step with your CRM. Use this when energy-data-driven leads generated in the Clevergy
  app need to land in a utility's own sales platform.
api: openapi/clevergy-connect-api-openapi.yml
generated: '2026-08-17'
method: generated
source: >-
  Grounded in https://docs.clever.gy/developer/how-to-set-up/sales-opportunities and
  https://docs.clever.gy/webhooks/sales-opportunity; every operationId below was verified present in
  openapi/clevergy-connect-api-openapi.yml.
operations:
  - getSalesOpportunities
  - getContractSalesOpportunity
  - patchContractSalesOpportunity
  - deleteSalesOpportunity
---

# Sync sales opportunities into a CRM

Base URL: `https://connect.clever.gy`. Auth: `clevergy-api-key` header.

## 1. Register a webhook endpoint

You do not do this via the API — there is no self-service webhook management operation. Write to
`soporte.clientes@clever.gy` to have a `sales-opportunity` endpoint configured.

Clevergy will `POST` to your URL. The only authentication mechanism offered is a query parameter on
your own callback URL:

```
https://mywebhook.com/endpoint?auth_token=abcd1234
```

There is **no HMAC signature header and no shared secret**, so you cannot cryptographically verify a
delivery. Mitigate: use a long high-entropy token, accept the callback only over TLS, allow-list
source IPs if Clevergy will give you a range, and treat the payload as untrusted until you have
re-read the object from the API in step 3.

## 2. Handle the three event types

| Event | Meaning |
|---|---|
| `CREATE` | A new opportunity is generated — e.g. a user starts a contracting flow from the app |
| `UPDATE` | An opportunity changed (usually a status change) |
| `DELETE` | An opportunity was deleted |

## 3. Always re-read from the API

Clevergy states the rule plainly: *"Webhooks are event notifications only. Sales opportunity data
must always be retrieved via the Connect API."*

Take the `id` from the payload, then:

- `GET /sales-opportunities/contract/{id}` → **getContractSalesOpportunity** — the full
  `ContractSalesOpportunity`, which carries `userId`, `houseId`, `invoiceId`, `invoiceAnalysisId` and
  `tariffId`.
- `GET /sales-opportunities` → **getSalesOpportunities** — the paged list, for reconciliation sweeps.
  Page-based pagination (`page`, `size`, `sort`, `direction`) returning
  `size` / `page` / `totalPages` / `totalElements` / `elements`. Note this differs from
  `getTenantHouses`, which is cursor-based.

## 4. Map the status lifecycle

`SalesOpportunitySummary.status` is a five-value enum. Map it into your CRM's stages, do not invent
intermediate states:

| Status | Meaning |
|---|---|
| `STARTED` | The user initiated the opportunity by showing interest in a product (e.g. via a recommender or banner in the app) |
| `POTENTIAL` | Clevergy proactively suggested it from energy data and user behaviour, with no direct user interaction |
| `FORMALIZED` | The user accepted a contract or proposal |
| `CONVERTED` | Payment received and the product/service is being delivered (e.g. contract signed) |
| `REJECTED` | Lost — the user declined or stopped responding |

`STARTED` and `POTENTIAL` are meaningfully different: one is user-initiated demand, the other is a
Clevergy-generated recommendation. Scoring them identically will distort your funnel.

## 5. Push status back

`PATCH /sales-opportunities/contract/{id}` → **patchContractSalesOpportunity**

Use it when your CRM is the system of record for outcome — e.g. your agent closes the deal, so you
move the opportunity to `CONVERTED` or `REJECTED` in Clevergy.

`DELETE /sales-opportunities/{id}` → **deleteSalesOpportunity** removes it entirely. Prefer a
`REJECTED` status over deletion so the funnel history survives.

## 6. Know the product scope

`product` is enumerated as `CONTRACT`, `SOLAR`, `HEATPUMP`, `BATTERY`, `DEVICES`, `EV`, `OTHER` —
but the docs state **only `CONTRACT` is currently supported**; the rest are reserved for future use.
Handle unknown values gracefully rather than switching exhaustively on today's set.

## Hazards

- No retry, ordering or delivery-guarantee semantics are published for webhooks. Assume
  at-least-once and out-of-order: make your handler idempotent on `id` + `updatedAt`, and run a
  periodic reconciliation with `getSalesOpportunities` to catch anything dropped.
- `patchContractSalesOpportunity` has no idempotency key. Because it is a PATCH to a known `id` it is
  naturally idempotent for a given target state — keep it that way by always sending an absolute
  status, never a relative transition.
- Errors: named 4xx use `{timestamp, status, error, path}`; the `default` catch-all uses
  `{code, message}`. See `errors/clevergy-problem-types.yml`.
