---
name: Show Autopass partner parking locations
description: >-
  Retrieve and display Autopass partner parking locations (POIs) with rates, opening
  hours, supported vehicle types and operator brand, for a car-owner app.
api: openapi/autopass-openapi.yml
operations: [listPois]
---

# Show Autopass partner parking locations

Use this to render the list/detail of parking locations where the user can pay with Autopass.

## Auth
1. Obtain an OAuth 2.0 bearer token from `https://api.autopass.xyz/oauth/token`
   (Client Credentials or Authorization Code; scope `openid`). See
   `authentication/autopass-authentication.yml`.
2. Send `Authorization: Bearer <token>` on every request.

## Steps
1. Call **`listPois`** — `GET https://api.autopass.xyz/pois`.
2. For each element in the `data[]` array, display the required fields:
   - `data[].name` (location name)
   - `data[].detail.address`
   - `data[].detail.openingTime.status` and `.period`
   - `data[].currentPrice.price` and `data[].currentPrice.chargeMode`
   - `data[].detail.priceDetail.current` / `.complete`
3. Optionally surface `data[].detail.spaceInformation.totalLots`,
   `data[].detail.telephone`, `data[].vehicleTypes`, `data[].detail.memos[]`, and
   `data[].detail.spaceBrandName` (operator brand — show a brand identifier when mixing providers).

## Rules
- Display the "powered by Autopass" mark on the location/payment screens (Autopass 識別規範).
- When multiple service providers' locations are shown, brand-identify each provider.
