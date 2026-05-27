# Extensions, MCP Apps, and Tasks

## Extensions become first-class

The 2026-07-28 release candidate formalizes extensions through SEP-2133.

According to the article, extensions now have:

- Reverse-DNS identifiers
- Negotiation through client and server capability maps
- Dedicated `ext-*` repositories
- Delegated maintainers
- Independent versioning from the core specification
- An Extensions Track in the SEP process

This gives new capabilities a way to mature outside the core specification.

## Official extensions in this release

The article identifies two official extensions in the release candidate:

1. MCP Apps
2. Tasks

## MCP Apps

MCP Apps are described under SEP-1865. They let servers ship interactive HTML interfaces that hosts render in sandboxed iframes.

### Implementation implications

- Tools declare UI templates ahead of time.
- Hosts can prefetch templates.
- Hosts can cache templates.
- Hosts can security-review templates before runtime.
- Rendered UIs communicate with the host over MCP’s JSON-RPC base protocol.
- UI-initiated actions follow the same audit and consent path as direct tool calls.

## Tasks

Tasks shipped as an experimental core feature in MCP 2025-11-25. In the 2026-07-28 release candidate, Tasks move into an extension.

### New lifecycle

The article describes a stateless-compatible Tasks lifecycle:

1. The client advertises support for the Tasks extension.
2. The server decides when a `tools/call` should run as a task.
3. The server returns a task handle.
4. The client drives task progress through extension methods.

### Mentioned task methods

- `tasks/get`
- `tasks/update`
- `tasks/cancel`

### Removed behavior

The article states that `tasks/list` is removed because it cannot be scoped safely without sessions.

## Migration note

Anyone who shipped against the 2025-11-25 experimental Tasks API should migrate to the extension lifecycle described in the release candidate.
