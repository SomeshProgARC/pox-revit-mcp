# Architecture

POX Revit MCP separates the public MCP surface from the Revit API owner.

## Components

`ChatGPT / Codex`

Calls the MCP server over HTTPS. It sees tool descriptors, tool input schemas,
read-only annotations, and structured tool results.

`POXBridge Gateway`

Hosts `/mcp`. It translates MCP JSON-RPC tool calls into POX bridge requests.
It must not load Revit DLLs or call the Revit API.

`POXBridge Desktop`

Supervises the local gateway and receives launch context from POX.

`POX Revit add-in`

Owns the Revit API boundary. It hosts the current-user named-pipe bridge and
executes all Revit API work through `ExternalEvent`.

`Autodesk Revit`

Provides the active document, active selection, warnings, categories, and
elements.

## Trust boundaries

- HTTPS boundary: ChatGPT to POXBridge Gateway.
- Local process boundary: POXBridge Gateway to POX.
- Revit API boundary: POX to Revit via `ExternalEvent`.

## Read-only policy

The current MCP tools are read-only. They may inspect the active document,
categories, elements, selection, and warnings. They must not mutate the model.

Future mutating tools need:

- explicit tool descriptors marked destructive where appropriate
- preview output
- user approval
- audit record
- rollback or recovery guidance where possible
