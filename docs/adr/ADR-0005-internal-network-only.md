# ADR-0005 — Internal Service Communication

## Status

Accepted

---

## Context

The Phantom services communicate exclusively with one another.

Exposing internal services publicly would unnecessarily increase the attack surface.

---

## Decision

Services communicate only through the private Docker bridge network.

No internal service ports are published externally.

Only explicitly required public entry points may be exposed.

---

## Consequences

Advantages:

- Reduced attack surface
- Simpler firewall configuration
- Better service isolation
- Clear network boundaries

Trade-offs:

- External access requires explicit configuration
- Remote debugging may require additional tooling

The security benefits outweigh these operational considerations.