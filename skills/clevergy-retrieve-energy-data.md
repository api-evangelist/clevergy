---
name: Retrieve a Clevergy customer's energy data
description: >-
  Read consumption, production, real-time power, appliance-level disaggregation and peer comparison
  for a customer's house. This is the read-only core of the Clevergy API and the safest surface to
  expose to an agent.
api: openapi/clevergy-connect-api-openapi.yml
generated: '2026-08-17'
method: generated
source: >-
  Grounded in https://docs.clever.gy/developer/how-to-set-up/obtain-energy-data; every operationId
  below was verified present in openapi/clevergy-connect-api-openapi.yml.
operations:
  - getUserSupplies
  - getEnergyByHouseId
  - getPowerByHouseId
  - getEnergyComparison
  - getDisaggregationByHouse
  - getHouseDetail
  - storeHouseEnergies
---

# Retrieve energy data

Base URL: `https://connect.clever.gy`. Auth: `clevergy-api-key` header.

## 1. Get the houseId

`GET /users/{userId}/supplies` → **getUserSupplies**

Everything below is house-scoped, not user-scoped. Resolve the `houseId` first. (`getUserHouses` is
deprecated; use this.)

`GET /houses/{houseId}/house-detail` → **getHouseDetail** gives the house's metadata and supply
points if you need the CUPS or address.

## 2. Energy over a time window

`GET /houses/{houseId}/energy` → **getEnergyByHouseId**

Query parameters across the energy/power family: `startDate`, `endDate`, `granularity`, `timeZone`,
`includeTimeSpanStart`, `includeTimeSpanEnd`, and `month` on some operations.

Two things to get right:

- **`timeZone` is an explicit parameter, not an assumption.** Do not let it default silently if you
  care about day boundaries — Spanish tariff periods are wall-clock, so an hour of drift moves
  energy between price periods.
- **`includeTimeSpanStart` / `includeTimeSpanEnd`** control boundary inclusion. Set them
  deliberately or you will double-count or drop the edge interval when paging through months.

The response covers solar production, grid export, house consumption, and energy charged to or
discharged from batteries.

## 3. Real-time power

`GET /houses/{houseId}/power` → **getPowerByHouseId**

`PowerItem` breaks out `smartDevices[]` and `energyCommunities[]` alongside the house totals — use
it for live dashboards rather than polling the energy endpoint.

## 4. Comparison and disaggregation

- `GET /houses/{houseId}/energy-comparison` → **getEnergyComparison** — the house against a peer
  baseline; this is what drives the `clevergy-house-comparison` microfrontend.
- `GET /houses/{houseId}/disaggregation` → **getDisaggregationByHouse** — appliance-level breakdown
  (`HouseDisaggregation.devices[]`), the data behind `clevergy-breakdown`.

## 5. Pushing your own readings in

`POST /houses/{houseId}/smartmeter` → **storeHouseEnergies**

Body carries `energies[]` (`EnergyEntry`). Use this when you hold the meter data yourself — e.g. an
FTP feed from a distributor — instead of letting Clevergy pull it via CUPS/Datadis. This is a write
and a `403` here means the house is not yours.

## 6. Where the data comes from

Energy data does not exist until a source is connected. In order of typical use:

1. **Distributor / CUPS** — Clevergy connects directly using the supply-point code. Automatic.
2. **Datadis** — the Spanish smart-meter data hub; requires the customer to authorize, which the
   `clevergy-integration-smartmeter` microfrontend exists to collect.
3. **Solar inverter** — see `clevergy-onboard-solar-client.md`.
4. **Your own feed** — `storeHouseEnergies` above.

If reads return empty rather than 404, the house exists but no source is connected yet. Check
`GET /connections` → **getConnection** and the house's integrations.

## Agent notes

All operations in steps 2–4 are `GET`, classified `connected` / `read` in
`agentic-access/clevergy-agentic-access.yml`, and are the right surface to hand an agent. Note there
is no `429` and no published rate limit (`rate-limits/clevergy-rate-limits.yml`) — an agent looping
over houses should self-throttle. Pagination differs by resource: `getTenantHouses` is
cursor-based, `getUsers` is page-based. See `conventions/clevergy-conventions.yml`.
