# ADR-0003 — Stateless Correlation

## Status

Accepted

---

## Context

The Hive Mind coordinator is responsible for routing events.

Historical storage is outside its responsibilities.

---

## Decision

The coordinator remains completely stateless.

No database is used.

No event persistence is implemented.

Correlation exists only during event processing.

---

## Consequences

Advantages:

- Simpler implementation
- No migrations
- Lower operational complexity
- Predictable behavior

Trade-offs:

- No historical event analysis
- No replay
- No recovery after restart

These limitations are acceptable for the current deployment.