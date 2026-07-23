---
name: Track an Autopass parking session and settle the order
description: >-
  Follow a license-plate parking session from entry to exit: read the in-progress
  session, react to entry/exit/billing webhooks, and pull the final order for billing
  and re-invoicing.
api: openapi/autopass-openapi.yml
operations: [getSiteSession, getOrder]
---

# Track an Autopass parking session and settle the order

Use this for the license-plate auto entry/exit (車辨自動進出場) flow.

## Auth
- OAuth 2.0 bearer token (`Authorization: Bearer <token>`), scope `openid`. See
  `authentication/autopass-authentication.yml`.

## Steps
1. On the **entry-notification** webhook, call **`getSiteSession`** —
   `GET https://api.autopass.xyz/site-sessions/{id}` — and show `startDate` (entry time)
   plus current rate from `listPois` (`data[].detail.priceDetail.current`) so the user
   can see parking duration and estimated fee before exit.
2. On the **exit-notification** / **transaction-billing-notification** webhook, call
   **`getOrder`** — `GET https://api.autopass.xyz/orders/{orderNumber}` — and read:
   - `site.name` (parking-site name)
   - `paymentUserIdentity.id` (license plate)
3. Bill the user yourself (partner-side), then display the order so the user can confirm.
4. On the **refund-notification** webhook, notify the user of the refund result.

## Rules
- License plate (`paymentUserIdentity.id`) is the join key across session and order.
- Re-invoicing must be completed within 30 days of the transaction.
- Do not let the user deregister a plate while a session is active.
- Webhook catalog: `asyncapi/autopass-webhooks.yml`; conventions: `conventions/autopass-conventions.yml`.
