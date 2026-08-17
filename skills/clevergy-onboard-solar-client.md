---
name: Onboard a Clevergy client with a solar installation
description: >-
  Connect a customer's solar inverter installation to their house so the Clevergy app shows
  production, consumption, real-time power and solar savings — the Plan Pro onboarding path. Use
  this when the customer has PV and you are integrating with installer credentials.
api: openapi/clevergy-connect-api-openapi.yml
generated: '2026-08-17'
method: generated
source: >-
  Grounded in https://docs.clever.gy/developer/how-to-set-up/create-solar-client and
  https://docs.clever.gy/developer/how-to-set-up/solar-api-integration; every operationId below
  was verified present in openapi/clevergy-connect-api-openapi.yml.
operations:
  - createUser
  - createElectricityContract
  - getInstallations
  - getUserSupplies
  - connectInstallation
  - updateInstallation
  - getHouseEquipments
---

# Onboard a solar client

Base URL: `https://connect.clever.gy`. Auth: `clevergy-api-key` header.

## 0. Decide which credential model applies

This is the fork that determines whether you can do this via API at all:

- **User credentials** — the customer must complete the inverter integration themselves, from the
  Clevergy app. You cannot drive this from the Connect API.
- **Installer credentials** — you set the integration up once, either from the Operations Portal
  or via the Connect API, and it covers your installations. This skill covers that path.

Supported inverter vendors appear as named slots on the `HouseIntegrations` schema: `huaweiB2C`,
`froniusB2C`, `smaB2C`, `sungrowB2C`, `wibeeeSolar`, `enodeB2C`, `shelly`. Per-vendor setup detail
is at https://docs.clever.gy/helpdesk/integrations/inverters.

## 1. Create the client with an electricity contract

Run `clevergy-onboard-client.md` first: **createUser**, then **createElectricityContract** with the
correct `cups`. Solar without a contract has nothing to compare against, so the euro and savings
figures will not render.

## 2. Find the installation

`GET /installations` → **getInstallations**

Search by `name` or `plantCode` (the identifiers your inverter portal uses). You need the
`installationId` from the result. There is a waiting period after an integration is created before
data appears — the length depends on the vendor, so poll rather than assuming a failure.

## 3. Resolve the houseId

`GET /users/{userId}/supplies` → **getUserSupplies**

Returns the customer's supply points and houses; pick the `houseId` you want the installation
attached to.

Do not use `getUserHouses` (`GET /users/{userId}/houses`) — it carries the OpenAPI `deprecated`
flag and **getUserSupplies** is its replacement.

## 4. Link the installation to the house

`POST /houses/{houseId}/installations` → **connectInstallation**

Body (`ConnectInstallationRequest`): `{ "installationId": "<from step 2>" }`

This is the step that makes inverter data visible to the customer in the app: energy production,
grid export, house consumption, real-time power and solar savings.

Use **updateInstallation** (`PUT /installations/{installationId}`) to amend installation metadata
afterwards.

## 5. Verify

- `GET /houses/{houseId}/equipments` → **getHouseEquipments** — confirms the inverter and any
  battery now resolve against the house.
- Then read telemetry per `clevergy-retrieve-energy-data.md`.

## Notes and hazards

- A `404 Installation not found` on step 4 usually means the waiting period has not elapsed, not
  that the installation is wrong.
- `403 Forbidden` on house-scoped writes means the house is not owned by your tenant.
- Battery **control** (as opposed to monitoring) has a further precondition — see
  `clevergy-schedule-battery.md`.
- To surface the whole flow to the customer instead of building it, the
  `clevergy-integration-solar-inverters` microfrontend does step 0's user-credential variant.
  See `components/clevergy-components.yml`.
