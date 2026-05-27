# Deprecations

The release candidate deprecates three core features under the new feature lifecycle policy described in SEP-2577.

## Deprecated features and replacements

| Deprecated feature | Replacement path |
| --- | --- |
| Roots | Tool parameters, resource URIs, or server configuration |
| Sampling | Direct integration with LLM provider APIs |
| Logging | stderr for stdio transports; OpenTelemetry for structured observability |

## Deprecation status

The article describes these as annotation-only deprecations. The methods, types, and capability flags continue to work in this release and in every specification version published within a year of it.

Removing any of these features requires a separate SEP under the lifecycle policy.

## Practical guidance

### Roots

Start moving implicit root assumptions into explicit tool parameters, resource URIs, or server configuration.

### Sampling

Plan direct integrations with LLM provider APIs rather than depending on MCP Sampling as a core protocol feature.

### Logging

For stdio transports, use stderr. For structured observability, use OpenTelemetry.
