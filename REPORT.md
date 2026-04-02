# Lab 8 — Report

Paste your checkpoint evidence below. Add screenshots as image files in the repo and reference them with `![description](path)`.

## Task 1A — Bare agent

<!-- Paste the agent's response to "What is the agentic loop?" and "What labs are available in our LMS?" -->

## Task 1B — Agent with LMS tools

<!-- Paste the agent's response to "What labs are available?" and "Describe the architecture of the LMS system" -->

## Task 1C — Skill prompt

<!-- Paste the agent's response to "Show me the scores" (without specifying a lab) -->

## Task 2A — Deployed agent

Nanobot startup log excerpt showing the gateway started inside Docker:

```
nanobot-1  | Using config: /app/nanobot/config.resolved.json
nanobot-1  | 🐈 Starting nanobot gateway version 0.1.4.post5 on port 18790...
nanobot-1  | 2026-04-02 19:12:12.361 | INFO     | nanobot.channels.manager:_init_channels:58 - Web Chat channel enabled
nanobot-1  | ✓ Channels enabled: webchat
nanobot-1  | ✓ Heartbeat: every 1800s
nanobot-1  | 2026-04-02 19:12:16.097 | INFO     | nanobot.agent.tools.mcp:connect_mcp_servers:246 - MCP server 'lms': connected, 9 tools registered
nanobot-1  | 2026-04-02 19:12:18.696 | INFO     | nanobot.agent.tools.mcp:connect_mcp_servers:246 - MCP server 'webchat': connected, 1 tools registered
nanobot-1  | 2026-04-02 19:12:18.697 | INFO     | nanobot.agent.loop:run:280 - Agent loop started
```

## Task 2B — Web client

WebSocket endpoint test response:

```terminal
$ uv run python - <<'PY'
import asyncio
import json
import websockets

async def main():
    uri = "ws://localhost:42002/ws/chat?access_key=msnk"
    async with websockets.connect(uri) as ws:
        await ws.send(json.dumps({"content": "What labs are available?"}))
        print(await ws.recv())

asyncio.run(main())
PY
```

Response:
```json
{"type":"text","content":"Currently, there are **no labs available** in the LMS system. The system
 is healthy but hasn't been populated with any labs yet.\n\nWould you like me to:\n1. Trigger a sy
nc pipeline to see if labs need to be fetched from an external source?\n2. Help you with something
 else related to the LMS?","format":"markdown"}
```

Flutter client accessible at `http://localhost:42002/flutter/` - returns valid HTML.

Nanobot logs show successful WebSocket connection handling:
```
nanobot-1  | 2026-04-02 19:13:10,468 INFO [nanobot_webchat.channel] [channel.py:140] - WebChat: new connection chat_id=ab2320d0-2426-4671-ab84-2b6f8cb30c6d
nanobot-1  | 2026-04-02 19:13:10.471 | INFO     | nanobot.agent.loop:_process_message:425 - Processing message from webchat:ab2320d0-2426-4671-ab84-2b6f8cb30c6d: What labs are available?
nanobot-1  | 2026-04-02 19:13:16.385 | INFO     | nanobot.agent.loop:_prepare_tools:253 - Tool call: mcp_lms_lms_labs({})
nanobot-1  | 2026-04-02 19:13:25.651 | INFO     | nanobot.agent.loop:_process_message:479 - Response to webchat:ab2320d0-2426-4671-ab84-2b6f8cb30c6d: Currently, there are **no labs available**...
```

## Task 3A — Structured logging

<!-- Paste happy-path and error-path log excerpts, VictoriaLogs query screenshot -->

## Task 3B — Traces

<!-- Screenshots: healthy trace span hierarchy, error trace -->

## Task 3C — Observability MCP tools

<!-- Paste agent responses to "any errors in the last hour?" under normal and failure conditions -->

## Task 4A — Multi-step investigation

<!-- Paste the agent's response to "What went wrong?" showing chained log + trace investigation -->

## Task 4B — Proactive health check

<!-- Screenshot or transcript of the proactive health report that appears in the Flutter chat -->

## Task 4C — Bug fix and recovery

<!-- 1. Root cause identified
     2. Code fix (diff or description)
     3. Post-fix response to "What went wrong?" showing the real underlying failure
     4. Healthy follow-up report or transcript after recovery -->
