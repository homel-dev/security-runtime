# Security Runtime Concept

## Ownership

Security Runtime owns shared security infrastructure.

Its responsibility includes the infrastructure mechanics required to provide
secrets, identity, cryptographic trust, authentication, authorization and
security telemetry as reusable runtime services.

## Security Runtime owns

- secret storage and delivery infrastructure;
- stable security API/gateway surfaces;
- cryptographic key and PKI infrastructure;
- identity-provider infrastructure;
- workload/service identity infrastructure;
- shared authentication infrastructure;
- infrastructure-level authorization and security policy;
- security-runtime telemetry.

## Security Runtime does not own

- application business authorization rules;
- application workflow semantics;
- prompts, model policy or AI routing;
- host storage and host bootstrap responsibilities;
- consumer retry/fallback policy;
- project-specific persistence semantics.

## Product boundary

The runtime must not become synonymous with any single vendor product.

Examples:

- Vault is the initial secret-management engine;
- VE4PB provides the upstream Vault architecture used by Security Runtime;
- Keycloak is a likely future IAM implementation, not yet an architectural
  requirement;
- Cilium is a candidate network/security substrate;
- Istio Ambient remains a possible later service-mesh implementation.

## Deployment sequencing

Secrets come first.

Service mesh, identity, PKI and other security domains are intentionally
separate follow-on phases so their requirements can be established from the
running secret-management platform rather than guessed in advance.
