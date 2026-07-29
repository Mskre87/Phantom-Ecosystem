# ADR-0001 — Event-Driven Architecture

## Status

Accepted

---

## Context

The Phantom Ecosystem consists of multiple specialized services responsible for different areas of security research.

These services must communicate while remaining as independent as possible.

Direct service-to-service communication would increase coupling and reduce flexibility.

---

## Decision

The ecosystem adopts an event-driven architecture.

Participating services publish events instead of invoking consumer services directly.

Event routing for the currently integrated producers and consumers is handled through Redis Pub/Sub and the Hive Mind coordinator.

---

## Consequences

Advantages:

- Loose coupling
- Independent services
- Easy expansion
- Simplified integration
- Independent deployments

Trade-offs:

- Asynchronous communication
- Event tracing becomes more important
- Producers are unaware of consumers

These trade-offs are acceptable for the current architecture.