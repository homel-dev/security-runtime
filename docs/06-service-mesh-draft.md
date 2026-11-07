# Service Mesh Draft

## Status

Architectural placeholder and research baseline.

No service-mesh implementation is selected or deployed by the initial Security
Runtime phase. This document preserves the current direction so the design is
not restarted from zero later.

## Purpose

The future mesh layer is expected to provide some combination of:

- transparent workload-to-workload encryption;
- workload identity and mutual authentication;
- infrastructure-level service authorization;
- traffic policy and controlled service-to-service reachability;
- security telemetry that can be consumed by the runtime observability stack.

The exact division of responsibilities between CNI, mesh, PKI and Security
Runtime remains an implementation decision.

## Current platform direction

Cilium is the preferred CNI/base networking candidate.

The first mesh/security path to evaluate is therefore Cilium-native
functionality rather than introducing a second dataplane by default.

As of the current Cilium stable documentation, Cilium supports ztunnel-based
transparent encryption as a Beta feature. The documented model provides
per-node ztunnel proxies and transparent L4 mTLS for enrolled namespaces.

The current documented limitations are material to the evaluation:

- enrollment is namespace-level rather than pod-level;
- only TCP traffic is currently covered by ztunnel mTLS;
- Cilium Cluster Mesh is currently incompatible with ztunnel mode;
- ztunnel encryption currently interferes with normal Cilium L4 NetworkPolicy
  enforcement because traffic leaves the pod encrypted.

These limitations prevent treating the current Cilium-native path as an
already-selected production mesh.

Official reference:
https://docs.cilium.io/en/stable/security/network/encryption-ztunnel/

## Trust and PKI boundary

The current Cilium ztunnel documentation provisions bootstrap and CA material
through Kubernetes Secrets.

That mechanism is not adopted as the Security Runtime trust architecture by
this draft.

Security Runtime is intended to own the broader trust/PKI boundary. The future
mesh design must therefore determine how workload identity and certificate
issuance integrate with the Security Runtime trust chain rather than silently
creating an unrelated root of trust.

The exact Vault/PKI integration is intentionally unresolved here.

## Istio Ambient fallback

Istio Ambient remains the primary fallback if Cilium-native functionality
cannot satisfy the required mesh semantics.

Istio officially documents operation with Cilium CNI. The current integration
requirements include configuring Cilium with `cni.exclusive=false`.

The official Istio documentation also records compatibility constraints,
including unsupported Cilium BPF masquerading for Ambient health-check behavior
and interactions between Cilium NetworkPolicy and kubelet health probes.

Official reference:
https://istio.io/latest/docs/ambient/install/platform-prerequisites/

## Desired deployment properties

Any selected mesh design should preserve these Security Runtime properties:

- consumers address stable runtime/service identities rather than mesh-specific
  implementation endpoints;
- mesh replacement must not require application-level URL or protocol changes
  where avoidable;
- workload identity and trust roots must have an explicit owner;
- secret/identity/PKI responsibilities must not be duplicated implicitly across
  Vault, mesh and Kubernetes;
- namespace enrollment and rollout must support progressive adoption;
- observability must integrate with the existing runtime-local telemetry model;
- failure of the mesh control plane should have explicitly defined effects on
  already-running data-plane traffic.

## Candidate evaluation order

1. Cilium as the base CNI.
2. Evaluate current Cilium-native transparent encryption / ztunnel capabilities
   against the required identity, policy, protocol and operational semantics.
3. If those semantics are insufficient, evaluate Istio Ambient on top of
   Cilium CNI.
4. Define the Security Runtime PKI/workload-identity integration before
   production mesh adoption.

This ordering is a design direction, not a production implementation decision.

## Explicitly unresolved

The following are not decided by this bootstrap architecture:

- final mesh implementation;
- final workload identity format;
- certificate issuer and rotation protocol for mesh identities;
- whether Vault directly participates in mesh certificate issuance;
- authorization-policy ownership split between Cilium/Istio and Security
  Runtime;
- multi-cluster mesh topology;
- exact gateway-to-mesh integration;
- final network-policy interaction model;
- migration plan for existing namespaces.

These require a separate evaluated design before implementation.
