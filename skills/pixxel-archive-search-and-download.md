---
name: Search the Pixxel STAC archive and download assets
description: Search the STAC-based imagery archive, order archive items, and deliver or download the resulting assets.
api: openapi/pixxel-openapi-original.json
operations: [ListCollections, ListItemInCollection, SearchItemInArchive, ListAssets, AOICreateDownload, GetDownloadStatus]
---

# Search the Pixxel STAC archive and download assets

Base URL `https://api.pixxel.space`. Authenticate with
`Authorization: Bearer <PERSONAL_ACCESS_TOKEN>`. The archive follows the STAC
(SpatioTemporal Asset Catalog) model.

## Steps
1. `ListCollections` (GET `/v0/archives/collections`) — browse available STAC collections.
2. `SearchItemInArchive` (POST `/v0/archives/search`) — search items by geometry, time range, and filters; or `ListItemInCollection` (GET `/v0/archives/collections/{cid}/items`).
3. `ListAssets` (GET `/v0/aois/{aoi_id}/assets`) — list the assets available for an AOI.
4. `AOICreateDownload` (POST `/v0/aois/{aoi_id}/downloads`) — request a download of the selected assets.
5. `GetDownloadStatus` (GET `/v0/aois/{aoi_id}/downloads/{download_id}`) — poll until the download is ready, then fetch the returned asset URLs.

## Rules
- Downloads are asynchronous: create the request, then poll `GetDownloadStatus` rather than expecting an inline payload.
- A 404 on a collection item means it is not in your accessible catalog; a 403 means a permission/role gap.
- Respect the returned `pagination` object when paging large item lists.
