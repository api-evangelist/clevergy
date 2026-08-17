---
name: Monitor and schedule a Clevergy-connected battery
description: >-
  Read a home battery's capacity, power limits and state-of-charge history, then program charge and
  discharge actions to shift consumption into cheap tariff periods. This is the only operation in
  the Clevergy API that actuates physical hardware — treat it accordingly.
api: openapi/clevergy-connect-api-openapi.yml
generated: '2026-08-17'
method: generated
source: >-
  Grounded in https://docs.clever.gy/developer/how-to-set-up/battery-management; every operationId
  below was verified present in openapi/clevergy-connect-api-openapi.yml.
operations:
  - getHouseEquipments
  - getTenantEquipments
  - getStorageEquipment
  - getStorageEquipmentSoc
  - scheduleStorageEquipmentAction
  - getPowerByHouseId
---

# Monitor and schedule a battery

Base URL: `https://connect.clever.gy`. Auth: `clevergy-api-key` header.

## Read this precondition first

Clevergy is explicit and it splits the API in two:

> Battery information (e.g. capacity, SOC history) is available for **all** users, but battery
> **control actions** (such as charging or discharging commands) are only available for users who
> have connected their system and granted permission via **Huawei OAuth** integration.

So monitoring works broadly; **control currently requires Huawei plus an explicit customer OAuth
grant.** Other inverter vendors are documented as "actively working to support" — not supported.
Never assume a schedule call will land just because monitoring returned data.

## 1. Find the equipment

`GET /houses/{houseId}/equipments` → **getHouseEquipments** — equipment attached to one house.
`GET /equipments` → **getTenantEquipments** — everything across your tenant (page-based pagination:
`page`, `size`, `sort`, `direction`).

Take the `equipmentId` of the storage unit.

## 2. Read static battery information

`GET /equipments/{equipmentId}/storage` → **getStorageEquipment**

`StorageEquipment` returns:

- `status` — `ACTIVE` | `INACTIVE`. Check this before scheduling anything.
- `ratedCapacity` — kWh
- `maxChargePower` — **W**
- `maxDischargePower` — **W**

Watch the units: capacity is in kWh but the power limits are in W, while your schedule request will
express dispatch power in kW. Convert deliberately.

## 3. Read state of charge history

`GET /equipments/{equipmentId}/storage/soc` → **getStorageEquipmentSoc**

Time-windowed with `startDate` / `endDate`. Use it to visualize SOC and to verify a scheduled action
actually executed.

`GET /houses/{houseId}/power` → **getPowerByHouseId** gives live charge/discharge power alongside the
rest of the house.

## 4. Schedule actions

`POST /equipments/{equipmentId}/storage/schedule` → **scheduleStorageEquipmentAction**

Body (`ScheduleStorageActionRequest`) carries `actions[]`, each a `ScheduleStorageActionItem`:

| Field | Required | Meaning |
|---|---|---|
| `date` | yes | UTC date-time, e.g. `2023-10-01T12:00:00Z` |
| `action` | yes | `CHARGE` \| `DISCHARGE` \| `PAUSE` |
| `targetSOC` | no | target state of charge, percent |
| `dispatchTimeMins` | no | dispatch time, minutes |
| `powerKWDispatch` | no | dispatch power, kW |

Execution semantics, per the docs: an action runs automatically at its `date` and **ends when the
target is reached or another action starts.** That means the schedule is a sequence, not a set of
independent jobs — a later action silently truncates an earlier one. Build the whole day's sequence
in one call and reason about it as a timeline.

Typical use: `CHARGE` during low-tariff hours, `DISCHARGE` during peak-price hours. Pair it with
`getContractEnergyPrices` or the tariff operations to know when those are.

## 5. Safety rules — read before automating

- **There is no idempotency key on this API.** A retried `POST` posts the actions again. Since a new
  action truncates the running one, a duplicate submission can cut a charge cycle short. Deduplicate
  client-side; do not rely on the API.
- `date` is **UTC**. Spanish tariff periods are wall-clock local, and Spain observes DST. Convert
  once, at the edge, and test across a DST boundary.
- This operation is classified `physical` in `agentic-access/clevergy-agentic-access.yml`, with a
  short token TTL and human-in-the-loop recommended. It moves energy in and out of a customer's
  battery, which has a cost and a warranty/cycle-life consequence. Do not expose it to an
  unsupervised agent.
- Verify, do not assume: after scheduling, read `getStorageEquipmentSoc` over the window to confirm
  the action ran. There is no delivery receipt or job status resource.

## What you can do end to end

Per Clevergy's own summary of the battery surface: monitor battery power in real time and
historically, read static battery information, retrieve SOC over a range, and program operations by
sending scheduled actions with a time and target SOC.
