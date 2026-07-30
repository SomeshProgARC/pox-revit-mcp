# POX Revit MCP

ChatGPT-compatible MCP bridge design for read-only Autodesk Revit access
through POX and POXBridge.

This repository tracks the public MCP-facing design and integration contract.
The active implementation currently lives in the NexaBIM monorepo under
`POX/` and `POXBridge/`.

## Goal

Let a user ask ChatGPT/Codex natural-language Revit questions such as:

```text
make list of all the doors with fire rating show table
```

The model should call an MCP tool, POXBridge should route the request to POX,
and POX should read the active Revit document on the Revit API thread through
`ExternalEvent`. The result returns to chat as both structured JSON and a
markdown table.

## Architecture

```text
ChatGPT / Codex
  -> HTTPS /mcp endpoint
  -> POXBridge Gateway
  -> current-user named pipe
  -> POX Revit add-in
  -> Revit ExternalEvent
  -> active Autodesk Revit document
```

Hard constraints:

- POX is the only Revit API owner.
- POXBridge must not load Revit DLLs.
- All Revit API work runs through POX on the Revit API thread.
- The current surface is read-only.
- Future write tools must preserve preview, approval, and audit controls.

## Current tools

- `pox_active_document_info`
- `pox_list_categories`
- `pox_query_elements`
- `pox_current_selection`
- `pox_warnings_summary`

The first concrete MVP query is Doors with Fire Rating filtering and these
columns:

- `ElementId`
- `Mark`
- `Type`
- `Level`
- `Fire Rating`
- `Width`
- `Height`

## ChatGPT connection

ChatGPT cannot directly call a private `127.0.0.1` URL from the web. For
development, run Revit and POXBridge locally, then expose the local POXBridge
gateway through a trusted HTTPS tunnel and connect ChatGPT to:

```text
https://<public-host>/mcp
```

See [docs/chatgpt-setup.md](docs/chatgpt-setup.md).

## License

MIT.
