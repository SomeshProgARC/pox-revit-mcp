# Implementation status

The working implementation currently lives in the private NexaBIM monorepo.

Implemented there:

- POX current-user named-pipe MCP bridge
- Revit `ExternalEvent` request execution
- POXBridge `/mcp` JSON-RPC endpoint
- ChatGPT-oriented tool metadata
- markdown table output plus `structuredContent`
- Doors + Fire Rating read-only query MVP
- packaged POXBridge launcher smoke test

Validated there:

- POXBridge release tests: 22/22 passing
- POX debug build: passing
- POXBridge desktop package build: passing
- packaged launcher smoke: passing
- root installer rebuild: passing

Still pending:

- one clean live Revit end-to-end smoke with a visible Revit 2025.4 session
- stable production HTTPS exposure strategy
- future write-tool approval and audit design
