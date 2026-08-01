---
name: Run Pixxel analytics — indices, visualizations, and insights
description: Create spectral indices and visualizations on an AOI, run analytics workflows/jobs, and generate model insights.
api: openapi/pixxel-openapi-original.json
operations: [GetAoiById, CreateIndex, CreateVisualization, listWorkflows, getWorkflowCostEstimate, createJob, getJobById, createInsights, listInsights]
---

# Run Pixxel analytics — indices, visualizations, and insights

Base URL `https://api.pixxel.space`. Authenticate with
`Authorization: Bearer <PERSONAL_ACCESS_TOKEN>`.

## Steps
1. `GetAoiById` (GET `/v0/aoi/{aoi_id}`) — confirm the AOI to analyze.
2. `CreateIndex` (POST `/v0/indices`) — define a spectral index (e.g. NDVI, MNDWI) expression.
3. `CreateVisualization` (POST `/v0/aois/{aoi_id}/visualizations`) — render the index as a visualization on the AOI.
4. `listWorkflows` (GET `/v0/workflows`) then `getWorkflowCostEstimate` (GET `/v0/workflows/{workflow_id}/estimate`) — pick a model workflow and preview its cost.
5. `createJob` (POST `/v0/workflows/{workflow_id}/jobs`) — run the workflow; `getJobById` (GET `/v0/workflows/{workflow_id}/jobs/{job_id}`) — poll to completion.
6. `createInsights` (POST `/v0/aoi/{aoi_id}/insights`) / `listInsights` (GET `/v0/aoi/{aoiId}/insights`) — generate and list AOI insights.

## Rules
- Always call `getWorkflowCostEstimate` before `createJob` — workflow runs are billable.
- Jobs and insights are asynchronous; poll `getJobById` / `listInsights` for status.
- Errors follow `{ "error": { "code", "message", "details } }`; handle 403 (permissions) and 409 (state conflict) explicitly.
