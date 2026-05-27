# Authorization Hardening

The release candidate hardens MCP authorization behavior to align more closely with OAuth 2.0 and OpenID Connect deployments.

## SEP references mentioned

- SEP-2468: Validate `iss` on authorization responses per RFC 9207.
- SEP-837: Declare OpenID Connect `application_type` during Dynamic Client Registration.
- SEP-2352: Bind registered credentials to the issuing authorization server’s issuer and re-register when resources migrate.
- SEP-2207: Document refresh token requests for OIDC-style authorization servers.
- SEP-2350: Clarify scope accumulation during step-up authorization.
- SEP-2351: Clarify `.well-known` discovery suffix behavior.

## Issuer validation

Clients must validate the `iss` parameter on authorization responses. The article frames this as a mitigation for mix-up attacks, especially relevant to MCP’s single-client, many-server deployment pattern.

The article also warns that clients are expected to reject responses that omit `iss` in a future version. Authorization servers should start supplying it now if they do not already.

## Dynamic Client Registration

Clients now declare their OpenID Connect `application_type` during Dynamic Client Registration. This avoids an authorization server defaulting desktop or CLI clients to `web` and then rejecting localhost redirect URIs.

## Credential binding

Clients bind registered credentials to the issuer of the authorization server that issued them. If a resource migrates between authorization servers, clients re-register.

## Refresh tokens and step-up behavior

The specification now documents refresh token requests for OIDC-style authorization servers and clarifies scope accumulation during step-up authorization.

## Implementation checklist

- [ ] Validate `iss` in authorization responses.
- [ ] Emit `iss` from authorization servers.
- [ ] Store client credentials scoped to issuer.
- [ ] Re-register on issuer migration.
- [ ] Declare OIDC `application_type`.
- [ ] Review localhost redirect behavior for desktop and CLI clients.
- [ ] Test refresh token flows.
- [ ] Test step-up authorization with accumulated scopes.
- [ ] Validate `.well-known` discovery suffix logic.
