# Agents24 skills

Last Updated: 2026-09-08

Build working products with Agents24 from your coding agent. The official `agents24-platform` skill helps your agent create resources, use the CLI, and integrate streaming chat into a new or existing app.

## Install

```bash
npx skills add agents24/skills --skill agents24-platform
```

Choose your coding environment when prompted. Restart or refresh its skill discovery if needed. Ask it to use **agents24-platform** to build your app.

For agents without a skill installer, download the repository and make the entire `skills/agents24-platform` folder available to the agent, including `references/`. Read `SKILL.md` first.

## Try it

> Use the agents24-platform skill to build a customer-support app. Ask me for my product, approved documentation, support policies, and escalation contact. Preserve my existing application if there is one. Use real Agents24 execution, show sources, and verify an answered question, an unknown answer, and a request for human help.

Configure your organization API key in a server-only, gitignored environment file. Do not paste it into this repository or a public prompt.

[Documentation](https://docs.agents24.dev) · [CLI](https://www.npmjs.com/package/@agents24/cli)

## Maintenance

This repository owns the public customer skill. Update it when a published Agents24 contract changes, check commands against released packages, and keep linked docs consistent. Platform source and its canonical specifications define supported behavior; this skill describes that behavior.

Before releasing a change, verify relative links, install the skill from a clean directory, and exercise the affected customer workflow. Do not copy private platform implementation, credentials, internal runbooks, or local workspace paths here.

The coding-agent skill is separate from runtime Skills, which are Instructions loaded by an agent you build on Agents24.

## License

MIT.
