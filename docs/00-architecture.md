# Security Runtime Architecture

## Status

Architecture/bootstrap.

This document defines the initial architectural boundary of `security-runtime`.
Implementation details that have not yet been selected are explicitly left open.

## Purpose

Security Runtime centralizes reusable security infrastructure while keeping
ultimate trust material outside the Kubernetes runtime that consumes it.

The target trust direction is:

Host root of trust
→ offline/bootstrap Vault chain
→ Security Runtime
→ online security services
→ runtime consumers.

The cluster is not the ultimate root of trust.

## Architectural principles

### External root of trust

The key material required to recover the highest Vault authority is stored on
the host system and is not stored in Kubernetes or in the repository containing
encrypted Vault state.

### Reconstructable runtime

Security Runtime services should be recoverable from external durable state.
Recovery should prefer reconstruction over repair of partially damaged
distributed state.

### Stable consumer boundary

Consumers should use stable Security Runtime service endpoints rather than
depending directly on individual Vault workers, identity servers, certificate
issuers, or future security backends.

### Replaceable implementations

Vault, Keycloak, Cilium, Istio and other concrete products are implementations,
not the architectural boundary itself.

### Local telemetry

Security Runtime owns its namespace-local telemetry collection. It does not
become the central telemetry collector for other runtimes.

## Initial component model

The initial implementation centers on a security gateway and the Vault
subsystem supplied by the private upstream VE4PB project.

Future components may add identity/IAM, PKI, workload identity and service-mesh
integration without changing the runtime ownership boundary.

## Non-goals for the initial phase

The initial phase does not deploy:

- a service mesh; its current architectural draft is documented separately;
- Keycloak;
- a cluster-wide PKI architecture;
- a replacement for every Kubernetes Secret consumer at once.

Those are separate implementation decisions.
