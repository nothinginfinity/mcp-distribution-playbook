# MCP 2026-07-28 Specification Release Candidate

A structured repository artifact generated from the parsed article:

**Source:** http://blog.modelcontextprotocol.io/posts/2026-07-28-release-candidate/  
**Article:** “The 2026-07-28 MCP Specification Release Candidate”  
**Published:** May 21, 2026  
**Authors:** David Soria Parra and Den Delimarsky

This folder is a repo-ready article package: implementation notes, migration guidance, examples, and metadata for teams evaluating or adopting the MCP 2026-07-28 release candidate.

## What changed

The release candidate introduces the largest MCP revision since launch. The main themes are:

- A stateless protocol core for ordinary HTTP infrastructure.
- Removal of the protocol-level initialize handshake and session ID.
- New request metadata patterns using `_meta`.
- Required HTTP routing headers for Streamable HTTP.
- First-class extensions with independent lifecycle and repositories.
- MCP Apps for server-rendered user interfaces.
- Tasks as an official extension rather than experimental core functionality.
- Authorization hardening around OAuth 2.0 and OpenID Connect deployments.
- Deprecation of Roots, Sampling, and Logging.
- Full JSON Schema 2020-12 support for tool schemas.
- Formal lifecycle and governance rules for future evolution.

## Repo map

| Path | Purpose |
| --- | --- |
| `docs/article-brief.md` | Executive summary of the parsed article |
| `docs/migration-checklist.md` | Practical checklist for migrating from MCP 2025-11-25 |
| `docs/protocol-changes.md` | Detailed notes on stateless MCP and HTTP behavior |
| `docs/extensions.md` | Notes on Extensions, MCP Apps, and Tasks |
| `docs/authorization.md` | Authorization changes and implementation implications |
| `docs/deprecations.md` | Deprecated features and replacements |
| `docs/sep-index.md` | SEP references extracted from the article |
| `examples/` | HTTP and JSON examples based on the article |
| `metadata/source.json` | Source and parse metadata |

## Key dates

- **Release candidate locked:** May 21, 2026
- **Final specification target:** July 28, 2026
- **Validation window:** 10 weeks for SDK maintainers and client implementers

## Intended use

Use this repo artifact to brief engineering teams, plan migrations away from session-bound Streamable HTTP behavior, track authorization/schema changes, and prepare SDK/server implementations for the final specification.

## Attribution

Generated from the parsed article at the Model Context Protocol Blog. This repository artifact is an implementation-oriented derivative summary and checklist, not the canonical specification.
