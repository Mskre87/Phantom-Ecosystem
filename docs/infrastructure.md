# Infrastructure

## Purpose

This document describes the infrastructure that supports the Phantom Ecosystem.

It defines the physical and logical components required to execute the ecosystem, including the host environment, container runtime, networking, service discovery, Redis deployment, storage strategy, runtime configuration, and infrastructure lifecycle.

Application architecture, event correlation, and module behavior are documented separately.

---

## Overview

Phantom Ecosystem is deployed on a deliberately minimal infrastructure designed to maximize operational simplicity while maintaining complete service isolation.

The active infrastructure consists of:

- One Contabo VPS
- Docker Engine
- Docker Compose
- One Docker bridge network
- One Redis instance
- Twelve Phantom service containers

No additional orchestration platforms, distributed infrastructure, managed services, or external message brokers are used.

The infrastructure is intentionally compact, allowing every service to execute within a single isolated environment while remaining loosely coupled through internal networking.

---

## Infrastructure Design Philosophy

The infrastructure follows a small set of architectural principles.

### Single-host deployment

The complete ecosystem operates on a single virtual private server.

This approach minimizes operational complexity while providing sufficient resources for all active services.

---

### Container-first architecture

Every Phantom service executes inside its own Docker container.

Containerization isolates:

- Runtime environments
- Python dependencies
- Native analysis tools
- Process lifecycles
- Module configuration

Each service can be updated, restarted, or replaced independently.

---

### Minimal infrastructure

The ecosystem intentionally avoids unnecessary infrastructure components.

The current deployment does not include:

- Kubernetes
- Docker Swarm
- Service meshes
- Managed Redis
- Distributed databases
- External orchestration platforms
- Load balancers

Only components required by the current implementation are deployed.

---

### Ephemeral by default

Infrastructure components are designed to avoid unnecessary persistence.

Containers are considered disposable.

Persistent storage is limited to operational data required by the individual modules.

---

### Internal communication

All service-to-service communication occurs inside the private Docker network.

Internal services are not exposed directly to the Internet.

---

### Automatic recovery

Every service uses:

```yaml
restart: unless-stopped
```

If the VPS restarts unexpectedly, Docker automatically restores the complete Phantom stack unless it was intentionally stopped by an operator.

---

## Host Infrastructure

The ecosystem currently runs on a single Contabo VPS.

The host provides:

- CPU resources
- Memory
- Local storage
- Network connectivity
- Docker runtime

All active Phantom services share the same host operating system while remaining isolated inside Docker containers.

The host itself does not execute Phantom modules directly.

---

## Docker Engine

Docker Engine provides the runtime environment for every service in the ecosystem.

Each module executes inside an isolated container with its own:

- Filesystem
- Process space
- Dependencies
- Runtime configuration
- Lifecycle

Containerization prevents one module from affecting another at the operating system level.

---

## Docker Compose

The complete infrastructure is managed through a single Docker Compose stack.

The Compose configuration defines:

- Redis
- The twelve Phantom services
- Internal networking
- Restart policies
- Build configuration
- Bind mounts
- Environment variables
- Service dependencies

The Compose file represents the operational source of truth for the running infrastructure.

---

## Container Organization

The active deployment contains the following containers.

| Container | Type |
|------------|------|
| redis | Infrastructure |
| phantom_core | Service |
| phantom_source | Service |
| phantom_binary | Service |
| phantom_crypto | Service |
| phantom_mobile | Service |
| phantom_ai | Service |
| phantom_pipeline | Service |
| phantom_dns | Service |
| phantom_supply | Service |
| phantom_graphql | Service |
| phantom_js | Service |
| phantom_correlation | Service |

Each service executes independently while sharing the same internal communication layer.

---

## Build Strategy

The infrastructure uses two different deployment strategies.

### Phantom services

Every Phantom module is built directly from its private source code using Docker Compose.

The Compose configuration uses:

```yaml
build:
```

pointing to the corresponding module directory.

This ensures that each deployment reflects the current implementation without requiring prebuilt images.

---

### Redis

Redis is the only service deployed from a public container image.

```yaml
image: redis:alpine
```

No custom Redis image is maintained.

---

## Internal Networking

All containers are attached to the Docker bridge network:

```text
phantom_net
```

This network provides:

- Internal communication
- Service discovery
- Container DNS
- Redis connectivity

Docker automatically assigns internal IP addresses and maintains DNS resolution between containers.

Services communicate using container names rather than fixed IP addresses.

For example:

```text
phantom_source
        │
        ▼
redis
```

No static IP assignments or custom IPAM configuration are used.

---

## Service Discovery

Docker provides automatic DNS resolution inside `phantom_net`.

Every container can reach another service using its Docker service name.

Examples include:

```text
redis
phantom_correlation
phantom_source
phantom_supply
```

This eliminates the need for manual network configuration.

---

## Redis Infrastructure

Redis provides the internal messaging layer of the ecosystem.

The deployed Redis instance serves exclusively as the Pub/Sub broker.

The infrastructure does not use Redis as:

- Primary storage
- Task persistence
- Cache
- Database
- Long-term event storage

Redis exists solely to transport events between active services.

---

### Redis Configuration

Redis is started using:

```text
redis-server --save 60 1
```

This enables periodic RDB snapshots when Redis state changes.

However, the Redis container does not mount a persistent host volume for its data directory.

As a result, snapshot files remain inside the container filesystem.

Removing the Redis container permanently deletes those snapshots.

From the host perspective, Redis behaves as an ephemeral service.

---

## Storage Strategy

The infrastructure intentionally minimizes persistent storage.

Most container filesystems are disposable.

Persistence is provided only through selected bind mounts.

---

### Configuration files

Configuration files are mounted into containers.

Typical examples include:

```text
targets.json
.env
```

This allows configuration changes without rebuilding container images.

---

### Module state

Some modules mount lightweight state files.

Examples include:

```text
state.json
```

These files prevent repeated processing after container restarts.

Module-specific state handling is documented separately.

---

### Evidence storage

Raw findings are exported through bind-mounted directories.

Example:

```text
loot/
```

Evidence survives container recreation because it is stored on the host filesystem.

---

### Disposable filesystems

Apart from explicitly mounted files and directories, every container filesystem is considered disposable.

Deleting and recreating a container restores a clean runtime environment.

---

## Runtime Configuration

Runtime configuration is supplied through Docker Compose.

Typical configuration includes:

- Environment variables
- Bind mounts
- Build instructions
- Restart policies
- Network membership

Operational secrets remain outside the public documentation repository.

---

## Restart Policy

Every service in the ecosystem uses:

```yaml
restart: unless-stopped
```

This guarantees automatic recovery after:

- VPS reboot
- Host maintenance
- Power interruption
- Docker daemon restart

Containers remain stopped only when explicitly stopped by an operator.

---

## Bind Mount Strategy

Bind mounts are used only where operational persistence provides value.

The infrastructure currently uses bind mounts for:

| Purpose | Example |
|----------|---------|
| Configuration | `.env`, `targets.json` |
| Runtime state | `state.json` |
| Evidence | `loot/` |

No bind mounts are used for general application storage.

---

## Network Exposure

The infrastructure intentionally exposes no Phantom service directly to the Internet.

The current Docker Compose deployment contains no published ports.

There are no active directives equivalent to:

```yaml
ports:
  - "8080:8080"
```

or

```yaml
ports:
  - "6379:6379"
```

All communication between services remains inside `phantom_net`.

---

## Internal Services

The following infrastructure services remain internal:

| Service | Internal Port |
|----------|---------------|
| Redis | 6379 |
| FastAPI (Phantom Correlation) | 8080 |

Neither service is publicly reachable.

---

## External Communication

Although the infrastructure does not expose inbound services, Phantom modules initiate outbound connections.

Typical outbound communication includes:

- Git providers
- Package registries
- Target systems
- DNS services
- Blockchain APIs
- Discord webhooks

The infrastructure follows an outbound-only communication model.

---

## Infrastructure Lifecycle

### Startup

When Docker Compose starts:

1. Docker creates the network.
2. Redis starts.
3. Phantom services start.
4. Modules initialize independently.

---

### Runtime

During execution:

- Services communicate internally.
- Redis transports events.
- Modules perform autonomous monitoring.
- Evidence is written through bind mounts when required.

---

### Restart

If Docker or the VPS restarts:

- Containers are recreated automatically.
- Mounted files remain available.
- Disposable container filesystems are recreated.
- Redis resumes with an empty runtime state.

---

### Shutdown

Stopping Docker Compose terminates every Phantom service.

Persistent bind-mounted files remain on the host.

Disposable container filesystems are removed when containers are deleted.

---

## Legacy Infrastructure

Previous deployments used host-level services running outside Docker.

Legacy artifacts remain under:

```text
infrastructure/legacy/
```

These files are retained only for historical reference.

The active deployment uses Docker Compose exclusively.

---

## Infrastructure Characteristics

The current infrastructure has the following characteristics.

### Containerized

Every Phantom component executes inside Docker.

---

### Single host

The ecosystem currently operates from one VPS.

---

### Lightweight

Only required infrastructure components are deployed.

---

### Internal-only

Service communication remains inside the Docker bridge network.

---

### Ephemeral

Containers are disposable by design.

---

### Self-recovering

Automatic restart is provided through Docker restart policies.

---

### Loosely coupled

Services communicate through Redis rather than direct inter-process dependencies.

---

## Current Limitations

The current infrastructure intentionally accepts several limitations.

- Single host deployment.
- Shared hardware failure boundary.
- One Redis instance.
- No distributed orchestration.
- No high availability.
- No public service endpoints.
- No persistent Redis storage.
- Disposable container filesystems.
- Bind mounts limited to operational persistence.
- No container autoscaling.
- No multi-host scheduling.

These limitations reflect the current implementation and should not be interpreted as planned functionality.

Future infrastructure evolution is documented in:

```text
ROADMAP.md
```

---

## Infrastructure Summary

```text
Contabo VPS
    │
    ▼
Docker Engine
    │
    ▼
Docker Compose
    │
    ▼
phantom_net
    │
    ├── redis:alpine
    │
    ├── 12 Phantom service containers
    │
    ├── Bind-mounted configuration
    ├── Bind-mounted state
    └── Bind-mounted evidence
```

The Phantom Ecosystem infrastructure prioritizes simplicity, isolation, and operational reliability.

Rather than relying on complex orchestration platforms, the ecosystem uses a minimal containerized environment where independent services communicate over an internal Docker network while remaining isolated from direct external access.

---

## Related Documentation

- [`index.md`](index.md) — Documentation index
- [`architecture.md`](architecture.md) — Ecosystem architecture
- [`deployment.md`](deployment.md) — Deployment topology
- [`hive-mind.md`](hive-mind.md) — Redis and Correlation Engine
- [`event-model.md`](event-model.md) — Event contracts
- [`security-model.md`](security-model.md) — Security boundaries
- [`modules.md`](modules.md) — Module catalog
- [`style-guide.md`](style-guide.md) — Documentation conventions