---
name: agents24-platform
description: Build customer apps and integrations with Agents24 using its official CLI, editable resource packages, and SDKs. Use for consuming Agents24, not developing the platform itself.
---

# Build with Agents24

Last Updated: 2026-09-08

Turn the customer's idea into a working product. Preserve an existing application's framework, authentication, and layout. For a new chat app, prefer `npm create agents24-app@latest` with the BFF integration.

Ask the essential product questions together: who uses it, which knowledge is approved, what it may do, and what a good result looks like. Use one concrete example to verify the result before expanding it.

## Read the relevant reference

- [Resource authoring](references/resource-authoring.md): create, validate, sync, and publish Agents, Instructions, knowledge, and tools.
- [Product surfaces](references/product-surfaces.md): generate an app, extend an existing interface, or share a hosted agent.
- [Server integration](references/server-integration.md): configure credentials, identity, and trusted Node/Python runtime.

Use [official docs](https://docs.agents24.dev) and installed package types for exact API signatures and schemas. Never invent exports or commands. If a required capability is unavailable in the installed release, report it instead of copying package internals.

## Working defaults

- Keep resource source in an editable Agents24 package. Use the CLI for resource lifecycle and the SDK for application runtime.
- Prefer the generated BFF for new web apps: the browser calls the application's backend, which keeps the organization key private.
- Use existing user authentication when available. The generated guest session is a starting point, not an authenticated customer account.
- Keep organization keys in protected, gitignored server environment files. Never put them in browser code, public environment variables, logs, responses, or screenshots.
- Organization identity comes from the API key. Do not ask for or expose a project ID.
- Runtime Skills inside an Agent are Instructions; they are not this coding-agent skill.
- Agent publishing does not deploy the customer's website. Apps Builder and platform-published Apps are separate products, outside this workflow.

## Complete the requested result

Inspect the target before remote changes. Apply/publish only the resources needed for the requested outcome and preserve unrelated resources. Keep missing provider credentials and external integrations explicit; never simulate successful real-world actions.

Use the maintained client and chat components for streaming, cancellation, thread history, attachments, and approvals. Keep product-specific presentation and authentication in the host.

Verify resource validation, app build, a real response, and thread restore. For knowledge-backed apps, verify a supported answer and an unknown answer; for actions, verify approval and failure behavior. Report what works, what was created, and remaining configuration without secret values.
