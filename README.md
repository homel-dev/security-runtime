# Security Runtime

`security-runtime` is the shared security infrastructure runtime for Homel projects.

It owns reusable security infrastructure that should not be duplicated inside
application, AI, or host-platform runtimes.

## Scope

Security Runtime is intended to own:

- secrets infrastructure and secret delivery;
- cryptographic trust and PKI infrastructure;
- authentication and identity infrastructure;
- authorization and security-policy infrastructure;
- stable security service ingress/gateway endpoints;
- runtime-local security telemetry.

The runtime is deliberately implementation-oriented but product-agnostic.
Concrete products are replaceable implementation choices behind stable runtime
contracts.

## Vault subsystem

Vault functionality is supplied by the upstream **VE4PB** project:

https://github.com/homel-dev/ve4pb

VE4PB is currently private. `security-runtime` consumes that subsystem rather
than embedding its architecture directly into this repository.

The Vault model is based on:

- an authoritative durable-write plane;
- horizontally scalable serving Vault workers;
- immutable published Vault state generations;
- disposable local session state where loss is acceptable;
- explicit handling of state whose loss has durable external side effects;
- external bootstrap/root-of-trust material outside Kubernetes;
- reconstruction from published state as the primary recovery mechanism.

See [Vault integration](docs/02-vault-integration.md).

## Runtime boundaries

`security-runtime` is separate from:

- Host Runtime: host/platform foundation and cluster-level infrastructure;
- LLM Runtime: inference, provider gateways, MCP and AI infrastructure;
- consumer projects: application policy, workflow semantics and business logic.

See [Runtime boundaries](docs/04-runtime-boundaries.md).

## Planned domains

The initial implementation focus is Vault-backed secret infrastructure.

Likely later domains include:

- identity and IAM, with Keycloak as a candidate implementation;
- PKI and certificate services;
- workload/service identity;
- service-mesh integration;
- additional security gateways and policy services.

Service mesh is intentionally not part of the first implementation phase.

## Documentation

- [Architecture](docs/00-architecture.md)
- [Security Runtime concept](docs/01-security-runtime-concept.md)
- [Vault integration](docs/02-vault-integration.md)
- [Trust and recovery](docs/03-trust-and-recovery.md)
- [Runtime boundaries](docs/04-runtime-boundaries.md)
- [Roadmap](docs/05-roadmap.md)
- [Service mesh draft](docs/06-service-mesh-draft.md)

**Status:** architecture/bootstrap.
