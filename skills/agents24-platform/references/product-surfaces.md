# Product surfaces

Last Updated: 2026-09-08

For a new web app, generate the official chat scaffold:

```bash
npm create agents24-app@latest support-app -- --integration bff --starter customer-support
```

Use Node.js 22 or 24. Read the generator's output and generated README for install and local-run commands. The BFF defaults to Next.js; Vite + Hono is also supported. Check `--help` against the installed version before selecting advanced flags.

The Customer Support starter contains an editable Agent package, an Instruction, an empty Knowledge Store, and ingestion/retrieval pipelines. It does not contain the customer's knowledge or credentials. Follow [resource authoring](resource-authoring.md) to configure, apply, test, and publish it.

Configure the server-only key locally. Run `agents24 setup ./agents24` when ready to publish and configure the generated app. Start the app with its package-manager `dev` command.

## Existing applications

Preserve the host's architecture. Select only the layer it needs:

| Need | Package |
| --- | --- |
| Server-backed browser transport | `@agents24/client/bff` |
| Direct deployment-scoped client | `@agents24/client` and `@agents24/client/browser` |
| Headless React state | `@agents24/react` |
| Complete chat interface | `@agents24/chat-react/scaffold` |
| Composable chat presentation | `@agents24/chat-react/ui` |
| Existing Vercel AI SDK UI | `@agents24/ai-sdk` |

Use current [SDK examples](https://docs.agents24.dev/sdk) and exported types. Let maintained components own sessions, streaming, reconnect, cancellation, and thread state. Keep citations, branding, authentication, and business-specific actions in the consuming app.

## Sharing and deployment

A Hosted Link shares a published Agent without hosting a website. For a custom web/native client, a Client Deployment provides scoped session credentials; its deployment ID is public, an organization key is not.

A generated BFF uses a fixed Agent set on the server. Development defaults to draft runtime; production uses published runtime. Direct Client Deployments use published Agents.

Publishing resources and deploying the website are separate. Configure production environment variables on the website host and replace guest identity when authenticated accounts are required. See [Deploy](https://docs.agents24.dev/deploy).

## Verify the experience

Build the app, send a real message, restore the conversation, and cancel a response. Verify knowledge answers against ingested sources. Exercise missing knowledge and an unavailable action honestly. Check that the client bundle contains no organization key.
