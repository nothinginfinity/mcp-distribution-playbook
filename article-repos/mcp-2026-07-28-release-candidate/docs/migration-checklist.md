# Migration Checklist: MCP 2025-11-25 to 2026-07-28 RC

## Protocol and transport

- [ ] Remove assumptions that every remote MCP interaction begins with `initialize` / `initialized`.
- [ ] Stop depending on `Mcp-Session-Id` for protocol-level session continuity.
- [ ] Ensure each request carries required protocol metadata.
- [ ] Add `MCP-Protocol-Version` to Streamable HTTP requests.
- [ ] Add `Mcp-Method` to Streamable HTTP requests.
- [ ] Add `Mcp-Name` where applicable.
- [ ] Reject requests where routing headers and JSON-RPC body disagree.
- [ ] Verify servers can process requests without sticky sessions.
- [ ] Test behind a round-robin load balancer.
- [ ] Replace hidden transport session state with explicit application handles.

## Client metadata

- [ ] Move client info into `_meta` on requests.
- [ ] Move client capabilities into `_meta` where required.
- [ ] Implement `server/discover` where clients need server capabilities up front.
- [ ] Preserve trace metadata in `_meta` using documented W3C Trace Context key names.

## Server-to-client requests

- [ ] Ensure server-initiated requests only occur while processing a client request.
- [ ] Replace long-lived SSE prompt flows with Multi Round-Trip Request behavior.
- [ ] Return `InputRequiredResult` when additional client input is needed.
- [ ] Include `requestState` sufficient for any server instance to resume the operation.
- [ ] Accept retried calls containing `inputResponses` and echoed `requestState`.

## Cacheability

- [ ] Add `ttlMs` to list and resource read results where freshness can be described.
- [ ] Add `cacheScope` to identify whether cached results can be shared safely.
- [ ] Update clients to respect cache freshness.
- [ ] Remove assumptions that SSE is the only way to learn a list changed.

## Extensions

- [ ] Represent extensions using reverse-DNS IDs.
- [ ] Negotiate extensions through client and server capability maps.
- [ ] Version extensions independently from the core specification.
- [ ] Track official and experimental extensions separately.

## Tasks

- [ ] Migrate away from the 2025-11-25 experimental Tasks API.
- [ ] Treat Tasks as an extension.
- [ ] Support task handles returned from `tools/call`.
- [ ] Implement or call `tasks/get`.
- [ ] Implement or call `tasks/update`.
- [ ] Implement or call `tasks/cancel`.
- [ ] Remove reliance on `tasks/list`; the article states it is removed because it cannot be scoped safely without sessions.

## MCP Apps

- [ ] Review UI templates before runtime when possible.
- [ ] Render server-provided HTML in a sandboxed iframe.
- [ ] Ensure UI-initiated actions flow through the same JSON-RPC audit and consent path as direct tool calls.
- [ ] Add prefetching and caching paths for declared UI templates.

## Authorization

- [ ] Validate the `iss` parameter on authorization responses.
- [ ] Begin supplying `iss` from authorization servers if missing.
- [ ] Declare OpenID Connect `application_type` during Dynamic Client Registration.
- [ ] Bind registered credentials to the issuing authorization server issuer.
- [ ] Re-register when a resource migrates between authorization servers.
- [ ] Review refresh-token request behavior for OIDC-style authorization servers.
- [ ] Review scope accumulation during step-up authorization.
- [ ] Confirm `.well-known` discovery suffix handling.

## Schemas and errors

- [ ] Support JSON Schema 2020-12 for tool `inputSchema` and `outputSchema`.
- [ ] Keep the `inputSchema` root constrained to `type: "object"`.
- [ ] Support `oneOf`, `anyOf`, `allOf`, conditionals, `$ref`, and `$defs` where applicable.
- [ ] Allow `structuredContent` to be any JSON value.
- [ ] Do not automatically dereference external `$ref` URIs.
- [ ] Bound schema depth and validation time.
- [ ] Update missing-resource error handling from `-32002` to JSON-RPC `-32602 Invalid Params`.

## Deprecated features

- [ ] Plan replacement for Roots using tool parameters, resource URIs, or server configuration.
- [ ] Plan replacement for Sampling using direct LLM provider API integration.
- [ ] Plan replacement for Logging using stderr for stdio transports or OpenTelemetry for structured observability.
- [ ] Treat these as annotation-only deprecations for this release, not immediate removals.
