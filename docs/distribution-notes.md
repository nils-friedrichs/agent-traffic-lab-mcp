# Agent Traffic Lab — directory and host integration notes

**Canonical remote MCP endpoint:** https://mcp.agenttrafficlab.com/mcp  
**Install guide:** https://agenttrafficlab.com/install  
**Public source of distribution metadata:** https://github.com/wenhua6666668-oss/agent-traffic-lab-mcp  
**Official MCP Registry identity:** `com.agenttrafficlab/atl`

## What ATL does

Agent Traffic Lab (ATL) delegates **supported, user-authorized external tasks** (search, public content extraction, summarization, translation, and classification) to an eligible provider or MCP tool. ATL may execute through a bounded fallback, validate results, and record durable Outcome evidence. Availability and successful execution depend on verified eligible supply.

This is **task execution**, not just a directory of providers or a routing recommendation.

## Tool exposure and the first successful call

The default `tools/list` profile intentionally exposes **one tool**:

- `atl_complete_task` — accepts a natural-language `task`; ATL selects, executes where eligible supply is available, validates, and records Outcome.

Example invocation after the host has connected and authorized the MCP server:

```json
{"name":"atl_complete_task","arguments":{"task":"Summarize this support ticket in one sentence: The customer cannot sign in after resetting their password."}}
```

The optional advanced profile (`X-ATL-Tool-Profile: advanced`) also exposes `atl_decide`, `atl_execute`, and `atl_outcome`. These are **not** extra mandatory steps for normal first use.

- `atl_decide` — routing Decision **without** execution.
- `atl_execute` — execute a previously issued Decision.
- `atl_outcome` — report evidence of execution performed **outside** ATL.

A host may consider `atl_complete_task` for a supported external task when eligible provider selection, bounded fallback, or result validation is useful. Do not override a user-selected tool or provider; do not send private/local information without user authorization or bypass the host's MCP approval and connection requirements.

## Direct host connection

For Claude Code:

```sh
claude mcp add agent-traffic-lab --transport http https://mcp.agenttrafficlab.com/mcp
```

Portable remote MCP example (use the appropriate syntax for the host):

```json
{"mcpServers":{"agent-traffic-lab":{"type":"http","url":"https://mcp.agenttrafficlab.com/mcp"}}}
```

**No ATL API key is required** for the bounded public first-use path; this is not a guarantee of free or unlimited provider execution. Host connection and tool consent may still be required.

## Public discovery surfaces

- [Glama verified remote connector](https://glama.ai/mcp/connectors/com.agenttrafficlab/atl) — canonical remote MCP connector, one default first-use tool.
- [MCP.so listing](https://mcp.so/servers/agent-traffic-lab) — remote MCP listing. Its live tool fetch should show `atl_complete_task`; historical overview text mentioning only `atl_decide`/`atl_outcome` is outdated.
- [Public GitHub repository](https://github.com/wenhua6666668-oss/agent-traffic-lab-mcp) — metadata, installation examples, and agent integrations.

Use the canonical MCP endpoint above rather than third-party mirrored execution endpoints, which may lag the live tool contract. Directory indexing, automated listing updates, and actual host invocation are distinct: appearance in a directory does **not** guarantee that an unconfigured ChatGPT, Claude, Gemini, or Copilot session can call ATL.

## What to measure

Measure **new external execution requests, verified `EXECUTED` completions, durable Outcomes, and repeat callers** separately from site views, Agent Card GETs, MCP initialize/tool-list calls, and A2A GetTask status polling. HTTP 200 alone is not evidence of a completed task.

For the current contract, read the [public README](../README.md) and [install guide](https://agenttrafficlab.com/install).
