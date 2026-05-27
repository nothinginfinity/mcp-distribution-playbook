# Article Brief

## Article

**Title:** The 2026-07-28 MCP Specification Release Candidate  
**Source:** http://blog.modelcontextprotocol.io/posts/2026-07-28-release-candidate/  
**Date:** May 21, 2026  
**Authors:** David Soria Parra, Den Delimarsky

## One-sentence summary

The MCP 2026-07-28 release candidate makes MCP stateless at the protocol layer, adds a first-class extension framework, graduates Tasks into an extension, introduces MCP Apps, hardens authorization, deprecates several core features, and establishes a formal feature lifecycle policy.

## Main takeaways

### 1. MCP becomes stateless at the protocol layer

The protocol no longer requires the `initialize` / `initialized` handshake or the `Mcp-Session-Id` header. Each request carries the information it needs, including protocol version, client info, and capabilities through request metadata.

### 2. Production HTTP deployment gets simpler

A remote MCP server can now sit behind ordinary HTTP infrastructure. The article emphasizes round-robin load balancing, request routing by `Mcp-Method`, and cacheable list responses as practical deployment wins.

### 3. State is moved into explicit application handles

Application state is not banned. Instead, servers should expose explicit handles such as `basket_id` or `browser_id` as normal tool arguments. The model can then reason about and pass those handles across calls.

### 4. Server-to-client flows are restructured

Server-initiated requests may only happen while the server is processing a client request. Multi Round-Trip Requests replace long-held SSE flows by returning an `InputRequiredResult` and letting the client retry with responses and request state.

### 5. Extensions become first-class

Extensions now have reverse-DNS identifiers, negotiated capabilities, independent versioning, delegated maintainers, and an Extensions Track in the SEP process.

### 6. MCP Apps and Tasks are official extensions

MCP Apps allow servers to ship sandboxed, server-rendered HTML interfaces. Tasks move from experimental core functionality into an official extension with a stateless-compatible lifecycle.

### 7. Authorization aligns more closely with OAuth and OIDC

The release candidate adds requirements and clarifications around issuer validation, OpenID Connect application type, credential binding, refresh tokens, step-up scope accumulation, and discovery behavior.

### 8. Roots, Sampling, and Logging are deprecated

These are annotation-only deprecations in this release. They remain present for now, but each has a recommended replacement path.

### 9. Tool schemas move to JSON Schema 2020-12

Tool `inputSchema` and `outputSchema` support full JSON Schema 2020-12 capabilities, with implementation cautions around external references, depth, and validation time.

### 10. Governance is formalized

The article frames this breaking change as a foundation for a more stable future. Future features can evolve through the lifecycle policy, extensions, and conformance-suite-backed SEPs.
