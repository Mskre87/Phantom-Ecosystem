# ADR-0004 — Docker Compose Deployment

## Status

Accepted

---

## Context

The ecosystem currently operates on a single VPS.

Container orchestration is required, but infrastructure complexity should remain low.

---

## Decision

Docker Compose is used as the deployment orchestrator.

Every Phantom service runs as an independent container connected through a private Docker bridge network.

---

## Consequences

Advantages:

- Simple deployment
- Reproducible environments
- Low maintenance overhead
- Easy local development

Trade-offs:

- Single-host deployment
- No built-in orchestration
- Manual horizontal scaling

The current operational requirements do not justify a more complex orchestration platform.