# Security Model

## Purpose

This document describes the security model implemented by Phantom Ecosystem.

It defines the trust boundaries, communication security, infrastructure isolation, secret management strategy, and security assumptions that protect the ecosystem itself.

This document does not describe offensive security capabilities or target-specific techniques implemented by individual Phantom modules.

---

## Overview

Phantom Ecosystem is designed around a simple security principle:

> **Everything is private unless explicitly exposed.**

The ecosystem minimizes its attack surface by combining:

- Internal-only service communication
- Container isolation
- No public service endpoints
- Stateless internal coordination
- Explicit trust boundaries
- External secret management

Security is achieved primarily through infrastructure isolation rather than complex access control mechanisms.

---

## Security Principles

The security model follows several core principles.

### Private by default

Internal services are not exposed to the public Internet.

---

### Least exposure

Only outbound connections required for module operation are permitted.

---

### Container isolation

Every Phantom service executes inside an isolated Docker container.

---

### Internal trust

Modules communicate only through the private Docker network.

---

### Stateless coordination

The Correlation Engine stores no persistent operational history.

---

### Secret separation

Operational secrets are never stored in the public documentation repository.

---

## Trust Boundaries

The current implementation defines three primary trust boundaries.

```text
Internet
        │
        ▼
Contabo VPS
        │
        ▼
phantom_net
        │
        ▼
Phantom Services
```

Only services inside `phantom_net` are trusted to participate in internal communication.

---

## Infrastructure Boundary

The Docker host forms the primary infrastructure boundary.

Everything inside the host is considered part of the trusted runtime environment.

This includes:

- Docker Engine
- Docker Compose
- Redis
- Phantom services
- Internal networking

External systems are never considered trusted.

---

## Network Boundary

The internal Docker bridge network:

```text
phantom_net
```

acts as the primary communication boundary.

All internal traffic remains inside this network.

No Phantom service is intended to receive unsolicited Internet traffic.

---

## Service Isolation

Each Phantom module executes inside its own container.

Container isolation separates:

- Runtime processes
- Python environments
- Dependencies
- Filesystems
- Configuration

A failure or compromise affecting one container does not automatically compromise the remaining services.

---

## Redis Security

Redis is deployed exclusively inside the private Docker network.

Characteristics:

- No public port exposure
- Internal service access only
- Pub/Sub transport
- No external clients

Redis is trusted only as an internal communication component.

It is not intended to serve external applications.

---

## FastAPI Security

Phantom Correlation includes an internal FastAPI ingress.

Endpoint:

```text
/api/v1/ingest
```

Authentication header:

```text
X-Hive-Secret
```

The endpoint currently remains internal to `phantom_net`.

It is not published through Docker Compose.

This allows the interface to be implemented without increasing the current external attack surface.

---

## Authentication Model

The current architecture performs authentication only where required.

### Internal Redis

Redis communication relies on network isolation rather than application-level authentication.

Only containers attached to `phantom_net` can reach the Redis service.

---

### FastAPI

Requests must include:

```text
X-Hive-Secret
```

Requests without the expected secret are rejected.

---

## Secret Management

Operational secrets are stored outside this repository.

Examples include:

- Discord webhook URLs
- API tokens
- Service credentials
- Authentication secrets
- Environment variables
- Internal keys

Secrets are injected at runtime.

They are never committed to the public repository.

---

## Environment Configuration

Configuration is supplied through mounted files and environment variables.

Typical examples include:

- `.env`
- API credentials
- Module configuration
- Authentication tokens

The documentation intentionally omits secret values.

---

## Outbound Communication

Modules initiate outbound connections when required.

Typical destinations include:

- Git providers
- Package registries
- Blockchain APIs
- DNS services
- Target systems
- Discord webhooks

Outbound communication is initiated only by the Phantom services.

---

## Inbound Communication

The active deployment exposes no public Phantom service.

Current infrastructure publishes:

- No Redis port
- No FastAPI port
- No module API
- No web interface

The external attack surface is intentionally minimized.

---

## Repository Separation

The public repository documents the architecture.

It does not contain:

- Operational source code
- Production credentials
- Deployment secrets
- Private infrastructure configuration
- Internal automation

Implementation repositories remain private.

---

## Operational Security

Operational configuration is intentionally separated from architectural documentation.

This repository should be considered descriptive rather than operational.

Possession of the documentation alone is insufficient to reproduce the production environment.

---

## Event Security

Events are exchanged exclusively through the trusted internal network.

Current implementation characteristics:

- Internal Pub/Sub only
- No external publishers
- No external consumers
- No Internet-accessible event broker

Only trusted services participate in event exchange.

---

## Correlation Security

Phantom Correlation performs only event evaluation.

It does not:

- Execute offensive workflows
- Maintain privileged state
- Store historical intelligence
- Persist event history

Its limited responsibilities reduce its security footprint.

---

## Storage Security

Persistent storage is intentionally minimized.

Bind mounts are limited to:

- Configuration
- Runtime state
- Evidence

Disposable container filesystems reduce long-term exposure of temporary runtime artifacts.

---

## Logging and Reporting

Each Phantom module reports independently.

Notifications are delivered through dedicated Discord webhooks.

Phantom Correlation reports only correlation decisions.

No centralized logging database exists.

---

## Security Assumptions

The current implementation assumes:

- The Docker host is trusted.
- Containers attached to `phantom_net` are trusted.
- Docker networking is functioning correctly.
- Secrets are managed securely outside the repository.
- Internal communication is isolated from public networks.

These assumptions define the operational security model.

---

## Current Limitations

The current implementation intentionally accepts several limitations.

- Single trusted host.
- Internal network trust.
- No mutual TLS between services.
- No service mesh.
- No distributed identity provider.
- No centralized secrets manager.
- No hardware-backed key storage.
- No persistent security audit database.

These limitations describe the implemented architecture.

Future security enhancements belong in:

```text
ROADMAP.md
```

---

## Security Summary

```text
Internet
        │
        ▼
Contabo VPS
        │
        ▼
Docker Engine
        │
        ▼
phantom_net
        │
        ├── Redis
        ├── Phantom Services
        └── Phantom Correlation
```

The security model of Phantom Ecosystem prioritizes isolation, minimal exposure, container separation, and private internal communication.

Rather than relying on complex security infrastructure, the ecosystem reduces its attack surface by exposing no public services and by separating operational secrets from architectural documentation.

---

## Related Documentation

- [`architecture.md`](architecture.md)
- [`deployment.md`](deployment.md)
- [`infrastructure.md`](infrastructure.md)
- [`hive-mind.md`](hive-mind.md)
- [`event-model.md`](event-model.md)
- [`modules.md`](modules.md)
- [`style-guide.md`](style-guide.md)