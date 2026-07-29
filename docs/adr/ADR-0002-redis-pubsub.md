# ADR-0002 — Redis Pub/Sub

## Status

Accepted

---

## Context

The ecosystem requires lightweight communication between services.

Persistent messaging is not currently required.

---

## Decision

Redis Pub/Sub is used as the communication layer.

Messages are delivered using a fire-and-forget model.

No persistence is expected.

---

## Consequences

Advantages:

- Very low latency
- Minimal infrastructure
- Simple deployment
- Easy maintenance

Trade-offs:

- No replay
- No delivery guarantees
- No persistence

These limitations are acceptable for the current implementation.