# ChatGPT setup

This MCP path is read-only. POX owns all Revit API calls and POXBridge only
exposes the MCP endpoint.

## Local flow

1. Open Revit with POX loaded.
2. In Revit, click `AI Bridge > POX Bridge`.
3. POX starts a current-user named-pipe bridge inside Revit.
4. POXBridge Desktop starts the local gateway.
5. Expose the local gateway over HTTPS with a trusted tunnel.
6. Connect ChatGPT to the public HTTPS `/mcp` URL.
7. Ask Revit questions in natural language.

## Development tunnel

Read the POXBridge gateway address from the launcher descriptor or logs. It
will look like:

```text
http://127.0.0.1:<port>/
```

Start an HTTPS tunnel to that local port:

```powershell
ngrok http <port>
```

Connect ChatGPT to:

```text
https://<public-host>/mcp
```

## Test prompt

```text
make list of all the doors with fire rating show table
```

Expected behavior:

- ChatGPT selects `pox_query_elements`.
- POXBridge routes the request to POX.
- POX reads Doors from the active Revit model through `ExternalEvent`.
- ChatGPT receives `structuredContent` plus a markdown table.

## Production note

For production, replace the development tunnel with a stable HTTPS endpoint
that preserves the local Revit trust boundary. Do not expose write tools until
preview, approval, and audit flows are implemented.
