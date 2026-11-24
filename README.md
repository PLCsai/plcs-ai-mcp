# PLCs.ai MCP Server

Connect Claude, Cursor, Codex, or any MCP-compatible agent to your PLC projects.

[PLCs.ai](https://www.plcs.ai) is the AI platform purpose-built for PLC engineers: upload an Allen-Bradley (Studio 5000), Siemens (TIA Portal) or CODESYS V3 project and understand, troubleshoot, document, and safely generate code for it in plain English. This MCP server gives any AI agent governed access to that same capability — your projects, live tag values, and analyses — with per-tool permission scopes.

**Remote server:** `https://mcp.plcs.ai/mcp` (Streamable HTTP · OAuth)
**Full documentation:** https://www.plcs.ai/developer/mcp

## Quick start

### Claude Code

```bash
claude mcp add --transport http plcs-ai https://mcp.plcs.ai/mcp
```

Then run `/mcp` and choose **Authenticate** to sign in with your PLCs.ai account.

### Claude Desktop

Settings → Connectors → Add custom connector → URL `https://mcp.plcs.ai/mcp` → Connect and sign in. For an on-prem appliance, see the setup guide: https://www.plcs.ai/developer/mcp

### Cursor / Codex / other MCP clients

See per-client guides: https://www.plcs.ai/developer/mcp — covers Cursor, Codex, and any client that speaks remote MCP or can run `mcp-remote`.

### Headless and automation

MCP signs in with OAuth only; it does not accept API keys. For scripts, CI and other headless use, call the [REST API](https://www.plcs.ai/developer) (`/api/v1`) with a `plck_` API key instead.

## What the agent can do

18 tools, permission-scoped per OAuth grant:

| Area | Tools |
|---|---|
| Discover | `plcs_health`, `plcs_list_projects`, `plcs_get_project`, `plcs_get_project_source` |
| Ask | `plcs_ask_project` — a cited answer from the PLCs.ai assistant, not raw source to reason over |
| Live plant data | `plcs_get_live_values`, `plcs_get_tag_history`, `plcs_request_live_tags`, `plcs_get_window_series` (an observation window's samples at its own rate) |
| Analyze | `plcs_get_project_analysis` (dead code, race conditions, missing handshakes, interlock audits, cycle-time bottlenecks, signal tracing), `plcs_get_project_devices` (the project's full device inventory, paged) |
| Propose | `plcs_propose_code` — a plan you approve, then a **reviewable proposal**; nothing is saved without review |
| Version & export | `plcs_save_version`, `plcs_stage_upload`, `plcs_export_plc`, `plcs_export_pdf`, `plcs_get_export`, `plcs_download_export` |

The agent reasons over projects your organization **already owns**. Onboarding a
machine is a deliberate act in the app or through a version-source connector, so
there is no create-a-project tool — an agent can read, ask, propose and save
versions, but never add a project.

## Example prompts

- "List my PLC projects and check which ones have live data connected."
- "Why won't Conveyor 3 restart after a stop? Trace the interlocks."
- "Read the latest analysis on the Palletizer project and summarize what's safe to remove."
- "What did tag `Motor1_Enable` do in the last hour?"
- "Draft a fix for the missing handshake between Conv001 and Rob010 — proposal only."

## Safety model

- Every tool call is authenticated and scoped to your organization; the credential *is* the tenant.
- Proposing code is two steps — a plan you approve, then the change — and it returns a proposal either way; persisting a new version is a separate, separately-scoped action.
- Customer data is never used to train AI models. [Data handling →](https://www.plcs.ai/data)

## Requirements

- A PLCs.ai account ([start a free trial](https://www.plcs.ai))
- For the `mcp-remote` bridge: Node 18+

---

© PLCs.ai · [Terms](https://www.plcs.ai/terms) · [Privacy](https://www.plcs.ai/privacy) · Questions: sales@plcs.ai
