# Vault Integration

## Upstream ownership

The Vault subsystem is implemented by the private upstream project:

https://github.com/homel-dev/ve4pb

Security Runtime consumes VE4PB as an upstream subsystem. The runtime repository
owns integration, deployment contracts and runtime exposure; VE4PB owns the
Vault-specific architecture and implementation.

## Consumer boundary

Consumers use the Security Runtime Vault endpoint rather than selecting an
individual Vault worker or authoritative writer.

The gateway is responsible for routing an operation to the correct Vault plane.

## Vault planes

### Authoritative plane

The authoritative Vault owns durable state that must survive destruction of the
serving fleet.

Examples include stable secrets, persistent policy, durable engine
configuration and rotations whose result must become part of the published
runtime state.

### Serving plane

The serving plane consists of independent Vault workers reconstructed from a
published Vault state generation.

Workers may hold local operational state when loss of that state is acceptable
under the policy of the corresponding operation.

The serving fleet can be horizontally scaled without making worker-local state
the durable source of truth.

### Operational state with external effects

State that looks ephemeral inside Vault but controls a durable external side
effect is not automatically disposable.

Such operations are routed according to their required persistence semantics.
When required, the authoritative plane owns their lifecycle.

## Publication

A durable authoritative mutation can produce a new published Vault generation.

The publication pipeline is conceptually:

authoritative mutation
→ Vault Raft snapshot
→ encryption
→ durable private storage
→ generation publication
→ progressive serving-fleet rollout.

Publication policy is an implementation policy. It may publish immediately,
coalesce a burst into a batch, or use another bounded strategy appropriate to
the workload.

## Runtime effect

A serving worker is replaceable.

Loss of a worker may invalidate only the operational state intentionally scoped
to that worker. Durable Vault state is recovered from a published generation,
not from that worker.
