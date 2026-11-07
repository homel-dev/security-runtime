# Runtime Boundaries

## Host Runtime

Host Runtime owns host/platform foundation and cluster-level infrastructure.

Examples include host-local foundational storage and cluster bootstrap/control
services.

## LLM Runtime

LLM Runtime owns AI infrastructure.

Its domain includes model inference, provider gateways, MCP-facing AI
infrastructure and related telemetry.

## Security Runtime

Security Runtime owns shared security infrastructure.

Its domain includes:

- secret management;
- security service ingress;
- trust and PKI infrastructure;
- identity infrastructure;
- shared authentication infrastructure;
- infrastructure-level authorization;
- security telemetry.

## VE4PB

VE4PB is not another Homel runtime.

It is the private upstream Vault subsystem used by Security Runtime.

This separation keeps Vault-specific implementation and lifecycle mechanics out
of the broader Security Runtime architecture and allows the runtime to contain
additional security domains later.

## Consumer projects

Consumers retain ownership of application meaning.

Security Runtime provides infrastructure capabilities; it does not decide
business authorization policy, workflow semantics or project-specific behavior.
