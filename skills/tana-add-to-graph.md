---
name: Add content to a Tana graph
description: Create nodes, fields, and supertags in a Tana Outliner workspace via the Input API, including tagged and typed content.
api: openapi/tana-input-api-openapi.yml
operations:
  - addToNode
---

# Add content to a Tana graph

Use the Tana **Input API** to push content into a Tana Outliner workspace graph.

## Prerequisites
- A workspace-scoped API token (generate in the Tana client under **API Tokens**).
- Send it as `Authorization: Bearer <token>`.

## Endpoint
`POST https://europe-west1-tagr-prod.cloudfunctions.net/addToNodeV2` — operation `addToNode`.

## Steps
1. Decide the destination. Set `targetNodeId` to a node id, `SCHEMA` (for supertag/field
   definitions), or `INBOX`. Omit it to place nodes in the Library. The relative Today-node
   is not supported.
2. Build a `nodes` array (max **100** nodes per call). Each node has a `name` and optional
   `description`, `supertags` (`[{id}]`), and `children`. Typed children use `dataType`:
   `plain`, `boolean`, `date`, `url`, `reference`, or `file`.
3. To create a **field definition**, target `SCHEMA` and set the node's supertag to `SYS_T02`.
   To create a **supertag**, set it to `SYS_T01`.
4. POST the payload. Keep the whole request under **5000 characters** and make at most
   **one call per second per token**.
5. On `200`/`201`, read the returned `nodeId`s so you can reference or extend those nodes later.

## Rules
- No idempotency key exists — retries can duplicate nodes; track returned `nodeId`s.
- Reading from the graph is not yet supported (roadmap).
- See `conventions/tana-conventions.yml`, `rate-limits/tana-rate-limits.yml`, and
  `errors/tana-problem-types.yml`.
