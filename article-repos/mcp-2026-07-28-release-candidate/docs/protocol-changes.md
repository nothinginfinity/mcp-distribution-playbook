# Protocol Changes

## Stateless core

The central architectural change is that MCP becomes stateless at the protocol layer. In previous Streamable HTTP behavior, clients established a session and then included `Mcp-Session-Id` on later calls. In the 2026-07-28 release candidate, each request is self-contained.

## Removed handshake

The article states that the `initialize` / `initialized` handshake is removed under SEP-2575. Information formerly exchanged at connection time now travels with requests.

Request metadata now carries information such as:

- Protocol version
- Client information
- Client capabilities

A new `server/discover` method lets clients fetch server capabilities when they need them before making other calls.

## Removed protocol-level session

The article states that the `Mcp-Session-Id` header and the corresponding protocol-level session are removed under SEP-2567. The operational result is that any MCP request can land on any server instance.

This removes the protocol-layer need for:

- Sticky routing
- Shared session stores
- Gateway body inspection to infer routing behavior

## Explicit application handles

The article distinguishes protocol statelessness from application statelessness. Applications can still maintain state, but the state should be represented explicitly through handles returned by tools and passed back as ordinary arguments.

Examples:

- `basket_id`
- `browser_id`
- `workspace_id`
- `job_id`

This makes state visible to the model rather than hidden in transport metadata.

## Server-to-client requests

Server-initiated requests are limited to the period while the server is actively processing a client request. This prevents user prompts from appearing without a user- or agent-initiated action.

## Multi Round-Trip Requests

Instead of holding a Server-Sent Events stream open for prompts, the server can return an `InputRequiredResult`. The client gathers user or agent input, then retries the original call with:

- `inputResponses`
- The echoed `requestState`

Because the continuation state is in the payload, any server instance can resume the operation.

## Routable HTTP

Streamable HTTP now requires routing headers:

- `MCP-Protocol-Version`
- `Mcp-Method`
- `Mcp-Name`

Gateways, load balancers, and rate limiters can route or classify traffic without inspecting the JSON-RPC body.

## Cacheable results

List and resource-read results now carry:

- `ttlMs`
- `cacheScope`

These fields tell clients how long a result remains fresh and whether it can be shared across users.

## Trace context

W3C Trace Context propagation in `_meta` is documented. The article specifically mentions fixed key names for:

- `traceparent`
- `tracestate`
- `baggage`

This allows distributed traces to follow calls across host applications, client SDKs, MCP servers, and downstream services.
