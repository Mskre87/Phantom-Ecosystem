# Deployment

## Purpose

This document describes the current deployment model of **Phantom Ecosystem**.

It defines the runtime topology, deployment boundaries, container organization, service relationships, network exposure, and operational characteristics of the active environment.

This document does not include source code, credentials, environment variable values, or provider-specific provisioning instructions.

---

## Overview

Phantom Ecosystem is deployed on a single Contabo VPS as one containerized Docker Compose stack.

The active deployment contains:

- One Contabo VPS
- Docker Engine
- Docker Compose
- One internal Docker bridge network
- One Redis service
- Twelve Phantom service containers

All active components run on the same host and communicate through the internal Docker network named:

```text
phantom_net
```

The current deployment is monolithic at the infrastructure level but modular at the application level.

Each Phantom service runs in its own container while remaining part of the same Compose deployment.

---

## Deployment Topology

```mermaid
flowchart TB

Internet

↓

Contabo["Contabo VPS"]

↓

Docker["Docker Engine"]

↓

Compose["Docker Compose"]

↓

Network["phantom_net"]

Network --> Redis[(Redis)]

Network --> Core

Network --> Source

Network --> JS

Network --> Binary

Network --> Crypto

Network --> Mobile

Network --> AI

Network --> DNS

Network --> Correlation

Network --> Supply

Network --> GraphQL

Network --> Pipeline
```

---

## Active Host

The complete production environment runs on a Contabo VPS.

The VPS provides the compute environment for:

- Container execution
- Internal service networking
- Redis Pub/Sub
- Module monitoring processes
- Analysis tools packaged within module containers
- Discord notification workflows
- Phantom Correlation

No active Phantom service is currently distributed across another VPS or host.

Remote and satellite agents are not part of the active deployment topology.

---

## Container Runtime

Docker Engine is the active runtime for all Phantom services.

Each module is packaged and executed as an isolated container.

This model separates:

- Runtime dependencies
- Python environments
- Native analysis tools
- Module-specific configuration
- Process lifecycles
- Failure boundaries

A failure or restart in one module container does not require the complete stack to share the same process lifecycle.

---

## Docker Compose Stack

The ecosystem is managed as a single Docker Compose deployment.

The active Compose configuration is located at:

```text
infrastructure/docker-compose.yml
```

Docker Compose is responsible for defining:

- The twelve Phantom services
- The Redis service
- The `phantom_net` network
- Service dependencies
- Container runtime configuration
- Internal service connectivity

The deployment is considered monolithic because the complete ecosystem is defined and managed through one Compose stack.

This does not make Phantom Ecosystem a monolithic application. The application architecture remains divided into independent specialized services.

---

## Active Services

The deployment contains the following Phantom containers:

| Container | Role |
|-----------|------|
| `phantom_core` | Web crawling and secret discovery |
| `phantom_source` | Source code event monitoring |
| `phantom_binary` | Binary monitoring and reverse engineering |
| `phantom_crypto` | Smart contract analysis |
| `phantom_mobile` | Android application analysis |
| `phantom_ai` | Machine learning repository analysis |
| `phantom_pipeline` | CI/CD workflow inspection |
| `phantom_dns` | DNS and subdomain monitoring |
| `phantom_supply` | Dependency ecosystem validation |
| `phantom_graphql` | GraphQL endpoint validation |
| `phantom_js` | Frontend JavaScript analysis |
| `phantom_correlation` | Event correlation and task delegation |

The stack also includes:

| Supporting service | Role |
|--------------------|------|
| `redis` | Ephemeral Pub/Sub transport |

---

## Internal Network

All Phantom containers and Redis are connected to:

```text
phantom_net
```

`phantom_net` is a Docker bridge network.

It provides:

- Internal service discovery
- Container-to-container communication
- Access to Redis
- Isolation from direct public access unless a port is explicitly published

Services can communicate using Docker service names within this network.

---

## Redis Connectivity

All twelve Phantom service containers are physically connected to the same Docker network as Redis.

This means every service has a network path to the Redis container.

However, network connectivity does not mean that every module currently uses Redis in its application logic.

### Active Redis participants

| Service | Current role |
|---------|--------------|
| `phantom_source` | Publishes Hive intelligence |
| `phantom_js` | Publishes Hive intelligence |
| `phantom_correlation` | Consumes intelligence and publishes delegated tasks |
| `phantom_supply` | Consumes Supply task events |
| `phantom_graphql` | Consumes GraphQL task events |

The remaining modules are connected to Redis at the infrastructure level but do not currently participate in the implemented Pub/Sub workflow.

---

## Runtime Communication

Internal event communication follows this deployment path:

```text
Producer container
        │
        ▼
phantom_net
        │
        ▼
Redis Pub/Sub
        │
        ▼
phantom_correlation
        │
        ▼
Module-specific Redis channel
        │
        ▼
Consumer container
```

The complete communication remains inside the Docker network.

Discord notifications leave the internal environment through outbound webhook requests initiated by the corresponding modules.

---

## Phantom Correlation Deployment

`phantom_correlation` runs as a container within the same Docker Compose stack.

The container includes:

- The asynchronous Correlation Engine
- The Redis Pub/Sub listener
- The FastAPI ingress service
- The Hive Mind Discord notification workflow

Phantom Correlation does not require a separate host or deployment stack.

---

## FastAPI Exposure

Phantom Correlation contains a FastAPI service listening internally on port:

```text
8080
```

The current Compose configuration does not publish this port to the public interface of the Contabo VPS.

The deployment does not contain an active mapping such as:

```yaml
ports:
  - "8080:8080"
```

Therefore, the FastAPI ingress is currently accessible only from within `phantom_net`.

The implemented endpoint is:

```text
/api/v1/ingest
```

Requests are authenticated using:

```text
X-Hive-Secret
```

The endpoint remains available as an internal ingress and as a prepared expansion point for future remote agents.

It is not currently exposed as a public Internet endpoint.

---

## External Communication

The active deployment uses outbound communication for module operations and Discord reporting.

Discord acts as the external notification layer.

### Module notifications

Individual modules report their findings to their own Discord webhooks.

### Correlation notifications

Phantom Correlation reports matched event chains to its dedicated Hive Mind Discord webhook.

### Internal task transport

Redis Pub/Sub is used for internal event and task transport.

Discord is not used to send commands between containers.

---

## Phantom Binary Deployment

`phantom_binary` runs exclusively as a Docker container in the current deployment.

Its active execution lifecycle is managed by Docker Compose.

The legacy file:

```text
phantom_bot.service
```

belongs to the previous host-based Oracle Cloud deployment model.

It is not used by the current Contabo deployment.

Any retained systemd material must be treated as legacy documentation or historical infrastructure rather than an active runtime component.

---

## Legacy Infrastructure

Previous deployment artifacts may remain under:

```text
infrastructure/legacy/
```

These files are not part of the current production topology.

Legacy artifacts must not be interpreted as active deployment mechanisms.

The current source of truth is:

```text
infrastructure/docker-compose.yml
```

---

## Deployment Boundaries

The current deployment has the following boundaries.

### Inside the Contabo VPS

- Docker Engine
- Docker Compose
- Redis
- `phantom_net`
- Twelve Phantom containers
- Internal FastAPI ingress
- Module runtime dependencies
- Correlation logic

### Outside the Contabo VPS

- Discord webhook destinations
- External services monitored or queried by individual modules

### Not currently deployed

- Public FastAPI ingress
- Remote Phantom agents
- Satellite VPS workers
- Multiple active Docker hosts
- A distributed Redis deployment
- A durable task broker
- A centralized database

---

## Deployment Characteristics

### Single-host topology

All active services share one VPS.

This simplifies:

- Stack management
- Internal networking
- Redis connectivity
- Container discovery
- Deployment coordination

It also means that the VPS is a shared infrastructure boundary for the complete ecosystem.

---

### Container isolation

Each Phantom service has its own container lifecycle.

Modules remain separated even though they share the same host and internal network.

---

### Shared communication service

Redis is shared by the event-participating modules.

Redis is deployed as part of the same Compose stack rather than as an external managed service.

---

### Stateless coordination

Phantom Correlation does not require persistent storage during deployment.

Restarting the Correlation container removes its volatile execution context.

---

### Ephemeral event delivery

Redis Pub/Sub does not retain events or tasks.

Deployment restarts can therefore cause messages to be missed when subscribers are unavailable.

---

## Startup Dependencies

The Docker Compose stack defines Redis as an infrastructure dependency for the Phantom services.

This provides the required network service ordering at the Compose configuration level.

However, a declared dependency does not provide durable message delivery or preserve messages while a subscriber is starting.

A service can be connected to Redis without actively publishing or consuming messages in its Python implementation.

---

## Restart Behavior

### Independent module restart

When an independent hunting module restarts:

- Its container process starts again.
- Volatile in-memory state is lost.
- Its own implementation determines whether any local state is restored.
- Other independent module containers can continue running.

### Phantom Correlation restart

When `phantom_correlation` restarts:

- Correlation history is not restored.
- Previous events are not replayed.
- Pending delegated tasks are not recovered.
- The service subscribes only to newly published events.

### Redis restart

When Redis restarts:

- Active Pub/Sub connections are interrupted.
- Ephemeral events are not restored.
- Subscribers must reconnect.
- There is no persistent event backlog.

### Consumer restart

When a consumer is unavailable during task publication:

- The task is lost.
- The task is not queued.
- Phantom Correlation does not retry it.
- No acknowledgement is expected.

---

## Failure Domains

The deployment contains several failure boundaries.

### Module container failure

A module failure primarily affects that module's own monitoring, analysis, and reporting workflow.

### Redis failure

A Redis failure interrupts all active Hive event transport and task delegation.

Modules that operate independently and report directly to Discord may continue their own workflows.

### Phantom Correlation failure

A Correlation failure disables event evaluation and task delegation.

Producer modules may continue their independent monitoring behavior, but Hive events published while Correlation is unavailable are lost.

### Host failure

Because all active services run on a single VPS, a host-level failure affects the entire ecosystem.

---

## Persistence Model

The deployment does not use Redis as persistent storage.

Phantom Correlation does not use:

- A database
- Persistent Redis keys
- A task queue
- A correlation history store
- A result store

Module-specific persistence behavior is outside the scope of this document and must be documented in the corresponding module documentation.

---

## Secrets and Runtime Configuration

Operational secrets are not stored in this public documentation repository.

Examples include:

- Discord webhook URLs
- API tokens
- `X-Hive-Secret`
- Service credentials
- Private target configuration
- Infrastructure access credentials

The implementation repositories and production environment maintain runtime configuration separately from the public architecture documentation.

Secret values must never be added to this repository.

Further security boundaries are documented in:

```text
docs/security-model.md
```

---

## Deployment Scope

This repository documents the deployment architecture without distributing operational implementation files or secrets.

Although the repository contains an `infrastructure/` directory, its purpose is to document the deployment structure and architectural organization.

The public repository is not intended to provide a complete operational environment capable of reproducing the private production deployment.

---

## Current Limitations

The current deployment model has the following documented limitations:

- All active services run on one VPS.
- The host is a shared failure boundary.
- Redis is a single shared communication node.
- Pub/Sub messages are not durable.
- Restarted consumers cannot recover missed tasks.
- Phantom Correlation does not recover previous state.
- FastAPI ingress is not publicly exposed.
- Remote agents are not currently part of the deployment.
- Most modules are connected to Redis but are not integrated at the application level.
- There is no multi-host orchestration.
- There is no centralized result persistence.
- Legacy systemd and Oracle deployment artifacts are not active.

These limitations describe the current deployment and do not represent unimplemented functionality as active behavior.

Planned changes belong in:

```text
ROADMAP.md
```

---

## Deployment Summary

```text
Contabo VPS
    │
    ▼
Docker Engine
    │
    ▼
Docker Compose
    │
    ├── Redis
    │
    ├── phantom_core
    │
    ├── phantom_source
    │
    ├── phantom_binary
    │
    ├── phantom_crypto
    │
    ├── phantom_mobile
    │
    ├── phantom_ai
    │
    ├── phantom_pipeline
    │
    ├── phantom_dns
    │
    ├── phantom_supply
    │
    ├── phantom_graphql
    │
    ├── phantom_js
    │
    └── phantom_correlation
            │
            ▼
       phantom_net
```

The active Phantom Ecosystem deployment is a single-host, containerized environment composed of isolated services connected through a shared internal network.

Docker Compose manages the complete stack, Redis provides ephemeral internal Pub/Sub communication, and Discord serves as the external notification layer.

---

## Related Documentation

- [`index.md`](index.md) — Documentation index
- [`architecture.md`](architecture.md) — Ecosystem architecture
- [`infrastructure.md`](infrastructure.md) — Container and network infrastructure
- [`hive-mind.md`](hive-mind.md) — Correlation and Redis communication
- [`event-model.md`](event-model.md) — Event and task contracts
- [`security-model.md`](security-model.md) — Deployment security boundaries
- [`modules.md`](modules.md) — Module catalog
- [`style-guide.md`](style-guide.md) — Documentation conventions