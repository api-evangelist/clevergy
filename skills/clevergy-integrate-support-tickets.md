---
name: Integrate Clevergy support tickets with a helpdesk or CRM
description: >-
  Receive ticket webhooks from the Clevergy app, read the full ticket and its comment thread, reply
  with attachments, and drive the status lifecycle from your own support platform.
api: openapi/clevergy-connect-api-openapi.yml
generated: '2026-08-17'
method: generated
source: >-
  Grounded in https://docs.clever.gy/developer/how-to-set-up/ticket-integration and
  https://docs.clever.gy/webhooks/ticket; every operationId below was verified present in
  openapi/clevergy-connect-api-openapi.yml.
operations:
  - getTicketDetails
  - updateTicket
  - addCommentToTicket
  - createCommentAttachmentSignedUrl
---

# Integrate support tickets

Base URL: `https://connect.clever.gy`. Auth: `clevergy-api-key` header.

## What this replaces

Customers raise tickets from the Help module in the Clevergy app rather than calling the utility.
Those tickets can be worked either in the Clevergy Operations Portal or — the point of this skill —
in the utility's own CRM/helpdesk via webhooks plus the Connect API.

## 1. Register a ticket webhook

Configured by Clevergy on request (`soporte.clientes@clever.gy`); there is no self-service webhook
operation. Same authentication caveat as every Clevergy webhook: the only mechanism offered is a
query-parameter token on your own callback URL, with no signature header. Verify by re-reading from
the API, not by trusting the body.

Events: `CREATE` (a new ticket) and `UPDATE` (any change, including a reopen).

## 2. Read the ticket

Take `ticketId` from the payload:

`GET /tickets/{ticketId}` → **getTicketDetails**

The `Ticket` schema is unusually well cross-referenced — it carries `tenantId`, `userId`, `houseId`,
`contractId`, `equipmentId` and `alertId`, plus `comments[]`. That means you can route a ticket by
what it is actually about: an `equipmentId` points at an inverter or battery, an `alertId` ties it to
a smart alert, a `contractId` to a billing question.

Per the docs the same rule applies as for sales opportunities: *"Ticket data must always be retrieved
via Connect API."* The webhook tells you something changed, not what it now is.

## 3. Reply

`POST /tickets/{ticketId}/comment` → **addCommentToTicket**

Body is a `CommentRequest`. The comment appears to the customer in the app.

### Attachments are a two-step

`POST /comments/{commentId}/attachment-upload-url` → **createCommentAttachmentSignedUrl**

Returns a `CommentAttachmentUrl` — a signed URL you then upload the file to. Same pattern as invoice
upload: the signed URL is short-lived, and if you never upload, nothing errors — the attachment
simply never appears. Verify the upload.

## 4. Drive the status

`PUT /tickets/{ticketId}` → **updateTicket**

Body (`UpdateTicketRequest`) requires `status`, one of:

`NEW` | `OPEN` | `PENDING` | `SOLVED` | `CLOSED`

Mirror your helpdesk's states onto these five; do not attempt values outside the enum.

## 5. Handle reopening

The documented reopen flow: when a ticket is marked resolved, the customer is asked whether the issue
is fixed. If they reply that it is **not** resolved, they can add more detail and **the status returns
to `PENDING`**. Once reopened, normal webhook and Connect API flows apply.

So `SOLVED` is not terminal. If your CRM treats a closed ticket as immutable, you need an explicit
reopen path or you will silently drop customer replies.

## Hazards

- No delivery guarantees are published for webhooks. Make the handler idempotent on `ticketId` and
  reconcile periodically.
- `addCommentToTicket` has **no idempotency key**. A retried POST posts the comment twice, visibly, to
  the customer. Deduplicate before sending, not after.
- `404` variants are specific enough to act on (`Not found` vs `Resource not found`); `403` means the
  ticket belongs to another tenant. See `errors/clevergy-problem-types.yml`.
