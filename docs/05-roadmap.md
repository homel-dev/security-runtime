# Roadmap

## Phase 1 — Vault foundation

- integrate the VE4PB upstream subsystem;
- establish the stable Security Runtime Vault gateway contract;
- implement authoritative and serving Vault planes;
- implement state-generation publication and recovery;
- establish runtime-local telemetry;
- reproduce and record end-to-end propagation and recovery benchmarks.

## Phase 2 — Secret consumer migration

- migrate selected existing stable secrets to the new runtime;
- define delivery contracts for different secret classes;
- remove legacy secret transport mechanisms only through separately approved
  migrations.

## Phase 3 — Identity and IAM

Evaluate and introduce shared identity/IAM infrastructure.

Keycloak is the current likely candidate but is not selected by this bootstrap
architecture.

## Phase 4 — PKI

Define certificate authority, issuance, rotation and workload identity
requirements.

No service-mesh-specific PKI design is assumed in advance.

## Phase 5 — Service mesh

Evaluate service-mesh requirements after the secret and identity foundations
exist.

Current direction:

- Cilium is the preferred CNI candidate;
- evaluate Cilium-native security/service-mesh functionality first;
- preserve the ability to use Istio Ambient over Cilium CNI if needed.

No mesh implementation is part of the initial Security Runtime phase.

See [Service mesh draft](06-service-mesh-draft.md) for the current constraints,
candidate paths and unresolved decisions.
