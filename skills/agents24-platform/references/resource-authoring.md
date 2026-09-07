# Resource authoring

Last Updated: 2026-09-08

Use Node.js 22 or 24 and the public CLI:

```bash
npm install --global @agents24/cli
agents24 package init ./agents24 --name "Support Agent"
agents24 package validate ./agents24
```

Use the generated package identity. A manifest requires `schema: agents24.package/v2`, a cryptographic `package_id`, a name, and resource declarations. Do not invent or reuse a package identity for an independent product. Use `agents24 fork <source> <target>` for an independent copy.

## Author the product

A package contains `agents24.yaml`, Workflow YAML, Agent Markdown, Instructions in `skills/*.md`, RAG pipelines, Stores, and optional Artifacts. Manifest resource keys are stable identities; keep them when moving files.

Start from the official customer-support template where appropriate. Inspect the generated files and current schemas before editing. Do not synthesize graph/operator fields from memory.

- [Manifest schema](https://docs.agents24.dev/schemas/resource-package/2.0/manifest.schema.json)
- [Agent schema](https://docs.agents24.dev/schemas/resource-package/2.0/agent.schema.json)
- [Workflow schema](https://docs.agents24.dev/schemas/resource-package/2.0/workflow.schema.json)
- [Resource packages](https://docs.agents24.dev/guides/move-resource-packages)

Use symbolic external requirements and relative local references. Keep credentials, platform UUIDs, policies, and publication state out of source. Declare a local Artifact callable with `kind: artifact`, a relative `uses` path, and its `export`; the manifest determines whether it supplies a Tool or Toolset.

## Sync and publish

Configure `AGENTS24_API_KEY` locally or use the CLI's masked prompt. Never pass a key as a command-line flag. The API defaults to `https://api.agents24.dev`; `AGENTS24_BASE_URL` is an optional override.

```bash
agents24 prepare ./agents24
agents24 plan ./agents24
agents24 apply ./agents24
agents24 status ./agents24
```

Prepare generates declared local secrets. Plan previews changes. Apply creates or updates drafts for the same organization/package identity; it does not publish. Inspect status for missing requirements and unhealthy callable attachments.

An empty Knowledge Store is not a populated knowledge base. Apply does not ingest documents. Run the ingestion pipeline explicitly with approved sources, then query for a known fact before testing the Agent.

After draft testing:

```bash
agents24 publish ./agents24
agents24 status ./agents24
```

Publish requires the synchronized source and promotes the current drafts. `agents24 setup ./agents24` combines apply, publish, and detected-app configuration; it is not website deployment.

`agents24 pull ./agents24` previews platform draft changes before writing local package files. Do not overwrite local edits blindly. `package import` clones resources; it does not reconcile the existing installation.

## Custom code

Use Hosted Artifacts for platform-executed source or Remote Artifacts for customer-hosted tools. `agents24 dev` starts or attaches local Remote Artifact servers and maintains the development relay; it does not apply drafts unless `--apply` is explicit. Publish requires a working production binding. See [Artifacts](https://docs.agents24.dev/artifacts).

For unresolved dependencies, inspect `agents24 resources list --help` and `agents24 link --help`. Select exact existing resources; do not widen key scopes or silently substitute an integration.
