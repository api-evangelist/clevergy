---
name: Onboard a Clevergy client with contracts and invoices
description: >-
  Create an end customer in Clevergy, attach an electricity or gas contract, and upload their
  invoices — the minimum viable onboarding for the Plan Starter tier. Use this when a utility or
  energy retailer needs a customer to appear in the Clevergy app with real billing data.
api: openapi/clevergy-connect-api-openapi.yml
generated: '2026-08-17'
method: generated
source: >-
  Grounded in https://docs.clever.gy/developer/how-to-set-up/create-client and
  https://docs.clever.gy/developer/how-to-set-up/invoices; every operationId below was verified
  present in openapi/clevergy-connect-api-openapi.yml.
operations:
  - createUser
  - createElectricityContract
  - createGasContract
  - createInvoice
  - assignExternalUserIdToUser
  - assignExternalContractIdToContract
---

# Onboard a Clevergy client

Base URL: `https://connect.clever.gy`
Auth: send `clevergy-api-key: <tenant key>` on every request. Server-side only — never from a browser.

## 1. Create the customer

`POST /users` → **createUser**

Body (`UserRegistration`): `name`, `surname`, `email`, `dni`, `phoneNumber`, `language` (BCP 47, e.g. `es-ES`).

Returns a `userId`. Keep it — everything else hangs off it.

- `409 Conflict` means a user already exists with that email. The body uses the `HttpErrorUserExists`
  schema (no `path` field). **There is no idempotency key on this API** — treat the 409 as your
  deduplication signal and look the user up rather than retrying.
- If your CRM is the system of record, call **assignExternalUserIdToUser**
  (`POST /integrations/users`) so you can later resolve by your own id, and filter `getUsers`
  by `externalUserId`.

## 2. Create at least one contract

Electricity: `POST /users/{userId}/electricity-contracts` → **createElectricityContract**
Gas: `POST /users/{userId}/gas-contracts` → **createGasContract**

Required on `ElectricityContractRequest`: `startDate`, `status` (`ACTIVE`|`INACTIVE`), `address`,
`cups`, `importTariffType`, `tariffName`, `tariffAccess`, `contractPower`.

`cups` is the Spanish universal supply-point code (e.g. `ES1234567890123456AB`). It is the key
Clevergy uses to reach the distributor and pull consumption automatically, so getting it right is
what makes energy data appear at all. A wrong or unknown CUPS surfaces as
`400 Bad request (e.g., house cups not found)`.

Returns a `contractId`. `409 Conflict` means the contract already exists — but only when you
supplied a contract number.

Do **not** use the operations under the `Contracts - Deprecated` tag
(`createHouseContract` and friends). They are the superseded house-scoped model. See
`lifecycle/clevergy-lifecycle.yml`.

## 3. Upload invoices — this is a two-step

`POST /contracts/{contractId}/invoices` → **createInvoice**

Required on `CreateOrUpdateInvoiceRequest`: `issueDate`, `invoicePeriodFrom`, `invoicePeriodTo`,
`type` (`ELECTRICITY`|`GAS`|`WATER`|`OTHER`), `cost`, `externalId`, `obtainMethod`.

The response contains **`fileUrl`** — a signed URL. **You must upload the invoice PDF to it within
15 minutes.** If you do not, the invoice is never processed and never becomes visible to the
customer, and you get no error telling you so. Always verify the upload succeeded.

For `ELECTRICITY` invoices, also send `powerCost`, `totalEnergyConsumption`, `energyCost`,
`totalEnergySurplus`, `surplusSaving`, `otherCost` and `discountSaving`. They are optional, but
they are what enables the invoice-breakdown view in the `clevergy-invoices` microfrontend — the
feature that reduces billing support tickets.

`409 Conflict` on this operation means an invoice with that `externalId` already exists **for that
user**, including on a different contract of the same user. The uniqueness scope is wider than you
would expect.

## 4. Getting energy data flowing

Creating the user and contract does not by itself produce consumption data. An integration must
supply it — in most cases Clevergy connects directly to the distributor using the `CUPS`. See
`clevergy-retrieve-energy-data.md`, or `clevergy-onboard-solar-client.md` if the customer has a
solar installation.

## Error handling

Two envelope shapes, and you must parse both:

- Named 4xx responses: `{timestamp, status, error, path}`
- The `default` catch-all: `{code, message}`

Full catalog: `errors/clevergy-problem-types.yml`. No `429` is declared anywhere and no rate
limits are published (`rate-limits/clevergy-rate-limits.yml`), so back off conservatively on your
own initiative.
