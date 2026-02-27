# MCP Foundry

**The open, community-driven registry for [Model Context Protocol](https://modelcontextprotocol.io) (MCP) tool servers.**

MCP Foundry catalogs, scans, and distributes MCP tools: the building blocks that let AI agents interact with the real world. Whether it's querying a database, managing GitHub issues, or automating a browser, every tool in the registry is transparent about its permissions and reviewed for safety.

---

## What is MCP?

The **Model Context Protocol** is an open standard created by Anthropic that gives AI assistants a universal way to call external tools. An MCP server exposes functions (called "tools") over JSON-RPC via stdio, and any compatible AI agent can discover and invoke them.

MCP Foundry exists to solve the discovery and trust problem: *How do you find tools? How do you know they're safe?*

## The Registry

Our registry is the canonical source of truth for MCP tool metadata. It currently tracks **62+ tools** across 8 categories:

| Category | Examples |
|---|---|
| **Developer Tools** | GitHub, Git, GitLab, Linear |
| **Databases** | PostgreSQL, MongoDB, SQLite, Neo4j, Supabase, Neon |
| **Cloud & Infrastructure** | Cloudflare, Vercel, Heroku, Kubernetes, Docker |
| **Browser & Web** | Playwright, Puppeteer, Firecrawl, Fetch |
| **Search & AI** | Brave Search, Exa, Perplexity, Tavily |
| **Productivity** | Notion, Todoist, Obsidian, Google Calendar |
| **Messaging** | Slack, Discord, Telegram |
| **Observability** | Sentry, Grafana, PagerDuty, Datadog |

**23 verified tools** have passed automated security scanning and maintainer review. The remaining community tools are cataloged with full permission transparency while awaiting verification.

## How It Works

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  Developer       │     │  MCP Foundry      │     │  AI Agent        │
│  submits tool    │────▶│  Registry          │────▶│  discovers &     │
│  via PR          │     │  scans & reviews   │     │  installs tools  │
└─────────────────┘     └──────────────────┘     └─────────────────┘
```

1. **Submit**: Fork the registry, add your `toolbox.toml` manifest to `submissions/`, open a PR
2. **Scan**: Our CI pipeline validates the manifest structure and runs security analysis
3. **Review**: Maintainers verify permissions, check for prompt injection patterns, and approve
4. **Publish**: The tool appears in the verified catalog, available to every agent and runtime

## Registry API

The catalog is served as static JSON at `registry.mcpfoundry.org`:

```
GET https://registry.mcpfoundry.org/verified.json     # Security-verified tools
GET https://registry.mcpfoundry.org/unverified.json   # Community tools (awaiting review)
```

Both endpoints support CORS, return `application/json`, and are cached at the edge with 5-minute TTL.

## Security Model

Every tool in the registry declares its permissions explicitly:

- **Filesystem**: Which paths it needs to read/write (or `false` for none)
- **Network**: Which domains/protocols it connects to (or `false` for none)
- **Processes**: Whether it spawns child processes

Verified tools are additionally scanned for:
- Prompt injection patterns in tool descriptions
- Overly broad permission requests
- Known malicious code signatures
- Mismatched declared vs. actual behavior

## Repositories

| Repo | Purpose |
|---|---|
| [**registry**](https://github.com/mcpfoundry/registry) | Tool catalog, submission pipeline, CI review workflow, API schema |

## Compatible Clients

Tools from the registry work with any MCP-compatible client, including [Claude Desktop](https://claude.ai/download), [Cursor](https://cursor.com), [Agent Toolbox](https://getagenttoolbox.com), [Windsurf](https://www.windsurf.com), and others.

## Contributing

We welcome contributions of all kinds:

- **Submit a tool**: [Open a PR](https://github.com/mcpfoundry/registry/pulls) with your `toolbox.toml` manifest
- **Report issues**: [File a bug](https://github.com/mcpfoundry/registry/issues) or security concern
- **Improve the registry**: Better scanning, more categories, documentation fixes

See the [registry README](https://github.com/mcpfoundry/registry#readme) for detailed submission guidelines.

## Links

- **Website**: [mcpfoundry.org](https://mcpfoundry.org)
- **Registry API**: [registry.mcpfoundry.org](https://registry.mcpfoundry.org)
- **MCP Spec**: [modelcontextprotocol.io](https://modelcontextprotocol.io)
