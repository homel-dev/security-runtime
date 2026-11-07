# Trust and Recovery

## Root-of-trust boundary

The ultimate recovery material is outside Kubernetes.

The intended chain is:

host-only recovery key material
→ encrypted offline Vault state
→ host-started bootstrap/unseal Vault
→ recovery of the next Vault authority
→ recovery/unseal of Security Runtime Vault infrastructure.

The upper bootstrap components do not need to remain continuously online.

## Durable backup model

Vault durable state is exported through the supported Vault Raft snapshot
mechanism.

The snapshot is encrypted before durable repository storage.

The encrypted snapshot and the key required to recover it must not be stored in
the same security domain.

The current design permits encrypted state to be stored in a private GitHub
repository while recovery key material remains on the host system.

## Failure model

### Serving worker loss

Recreate the worker from the current published generation.

Worker-local disposable sessions may require client re-authentication.

### Serving fleet loss

Recreate the fleet from the current published generation.

### Security Runtime cluster loss

Recover the Vault authority from external durable state and reconstruct the
runtime.

### Authoritative Vault loss

Reconstruct the authority from its externally preserved state using the
bootstrap trust chain.

## RPO and RTO

RPO is defined by the latest successfully published durable generation.

RTO consists of the required bootstrap, decrypt, restore, unseal and serving
rollout path.

Numerical RPO/RTO targets are not defined in this bootstrap architecture.
They must be established from measured implementation behavior.

## Existing proof-of-concept observation

A previous proof of concept reportedly measured propagation from authoritative
write to refreshed serving Vault workers in the range of tens of seconds on
normal hardware.

That observation is historical input, not a benchmark claim of this repository.
The implementation must reproduce and record its own measurements.
