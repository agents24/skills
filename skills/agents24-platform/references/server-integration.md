# Server integration

Last Updated: 2026-10-06

Use a trusted backend when it owns authentication or needs organization-level API access. Node uses `@agents24/node`; Python uses `agents24`.

```ts
import { Agents24 } from "@agents24/node";

const agents24 = new Agents24({
  baseUrl: "https://api.agents24.dev",
  apiKey: process.env.AGENTS24_API_KEY!,
});
```

The trusted SDK requires an explicit API base URL. Use `https://api.agents24.dev` for the public platform. Read installed package types and [Node](https://docs.agents24.dev/sdk/node) or [Python](https://docs.agents24.dev/sdk/python) docs for exact calls.

## Browser-facing backend

Prefer the generated BFF for a new app. For an existing Node app, use `createAgents24BffHandler` from `@agents24/node/bff` and its fixed Agent configuration. Resolve the principal from the host's authenticated session, not user IDs submitted by the browser.

The browser uses `createBffAgentClient` from `@agents24/client/bff` against the same-origin route. Keep the key, Agent selection, and runtime authority on the server. Do not expose the handler as an unrestricted proxy.

The generated guest session uses its own `SESSION_COOKIE_SECRET`. Preserve that value locally and replace the guest identity resolver when real customer accounts are required.

## Server-owned execution

Use the embedded runtime when the server owns requests, streaming, and thread lifecycle. Use its attach, resume, cancel, and history methods rather than replaying the original prompt after a disconnect. A disconnected stream is not a cancelled run.

A Client Deployment is a different integration: backend exchange authenticates a user on the server and returns only scoped deployment-session material. Never return an organization key to the client.

## Resource authoring

Use source packages and the CLI for resources maintained in Git. Use SDK builders only when resource creation itself must happen programmatically. Query current catalogs and schemas before constructing low-level graphs. Validate drafts before publishing.

## Managed ingestion

Check the [capability catalog](https://docs.agents24.dev/sdk/capabilities) and installed package types before using `pipelines`, Node `ingestionJobs` / Python `ingestion_jobs`, and Node `knowledgeStores` / Python `knowledge_stores`. Use the first supporting Node/Python versions recorded in the catalog; do not substitute console routes or handwritten HTTP wrappers when the installed SDK lacks them.

Provision and publish pipelines through resource packages/CLI, then use supported application methods to discover the published executable, inspect its runtime input schema, upload, start, poll, read safe results/batches, resume, and cancel. Organization keys require `pipelines.read` for discovery/job reads, `pipelines.write` for uploads/commands, and the corresponding knowledge-store read/write scopes. They derive workspace identity without a project selector or user session.

Persist the start body and an explicit idempotency key before submission. Reuse both after uncertain delivery. Start/resume/cancel deduplicate admission for seven days; this does not promise exactly-once provider effects. Uploads accept 25 MiB and references expire after 24 hours. Preserve source data for a fresh submission after expiry. Resume only failed, partially failed, or cancelled jobs after fixing the external condition; print safe failure identifiers, not raw diagnostics. Store archive is a soft lifecycle operation. Document listing, replacement, and deletion remain separate.

Use the [complete managed ingestion example](https://docs.agents24.dev/sdk/managed-ingestion) for the source package and Node and sync/async Python execution, including failure and recovery. Verify with a dedicated organization key and disposable resources.

## Verify

Check authenticated users cannot access each other's conversations, credentials remain server-only, and a restored thread continues through the maintained SDK. Test a denied request and a runtime failure as well as a successful response.
