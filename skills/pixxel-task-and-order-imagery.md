---
name: Task and order Pixxel satellite imagery
description: Define an area of interest, check available satellites/bandsets, and submit a tasking order for new hyperspectral/multispectral capture.
api: openapi/pixxel-openapi-original.json
operations: [ListProjects, AOICreateAOI, "List Satellites", ListBandsets, SubmitOrder, ListOrderItems, GetOrderById]
---

# Task and order Pixxel satellite imagery

Base URL `https://api.pixxel.space`. Authenticate every request with
`Authorization: Bearer <PERSONAL_ACCESS_TOKEN>` and `Content-Type: application/json`.
The token is scoped by organization, project, and role.

## Steps
1. `ListProjects` (GET `/v0/projects`) — pick the project the order will belong to.
2. `AOICreateAOI` (POST `/v0/aois`) — create the area of interest (GeoJSON polygon) to be captured.
3. `List Satellites` (GET `/v0/satellites`) and `ListBandsets` (GET `/v0/bandsets`) — confirm the available satellites and spectral bandsets for the AOI.
4. `SubmitOrder` (POST `/v0/orders`) — submit the tasking order referencing the AOI, satellite, and bandset.
5. `ListOrderItems` (GET `/v0/order-items`) / `GetOrderById` (GET `/v0/orders/{id}`) — poll order and item status until fulfilled.

## Rules
- No idempotency key is supported; do not blindly retry `SubmitOrder` on a timeout — first re-list orders to check whether it was created.
- Errors return `{ "error": { "code", "message", "details } }`; a 403 means the token lacks the role/project permission for that order.
- Pagination on list endpoints uses `offset`/`limit` (or `page_num`/`page_size`); read the `pagination` object in the response.
