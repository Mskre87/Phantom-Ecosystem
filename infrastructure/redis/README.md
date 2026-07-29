# Redis

> Redis Pub/Sub communication layer for the Phantom Ecosystem.

---

# Overview

Redis acts as the internal communication layer between the autonomous services that compose the Phantom Ecosystem.

Rather than serving as a persistent datastore, Redis is used exclusively for event distribution through the Publish/Subscribe (Pub/Sub) messaging model.

---

# Purpose

The Redis instance provides a lightweight mechanism for exchanging events between services while maintaining loose coupling across the ecosystem.

Its responsibilities include:

- Event publication
- Event subscription
- Inter-service communication
- Lightweight event distribution

Redis is **not** responsible for event processing, persistence, or orchestration.

---

# Communication Model

The ecosystem follows a fire-and-forget communication model.

When a service publishes an event:

1. The event is published to a Redis channel.
2. All subscribed services receive the event.
3. Each subscriber independently decides whether to process it.
4. The publisher never waits for acknowledgements or responses.

This model minimizes dependencies between services and enables asynchronous execution.

---

# Current Usage

The documented architecture currently uses Redis for:

- Hive event publication
- Task delegation
- Internal service communication

Redis is not used for:

- Persistent storage
- Message queues
- Streams
- Event replay
- Scheduling
- Workflow orchestration

---

# Persistence

Redis persistence is not part of the documented architecture.

Although the Redis container may technically generate temporary RDB snapshots, the deployment does not mount persistent storage for Redis data.

The ecosystem does not rely on Redis persistence for normal operation.

---

# Network

Redis is deployed as an internal service within the Docker Compose stack.

Characteristics:

- Internal Docker bridge network
- No public exposure
- No external clients
- Accessible only by Phantom services

---

# Design Principles

The communication layer follows these principles:

- Event-driven architecture
- Loose coupling
- Stateless communication
- Lightweight messaging
- Independent service execution

---

# Public Repository Scope

This directory documents the Redis communication model only.

It does not include:

- Redis configuration files
- Authentication secrets
- Runtime configuration
- Production deployment assets

These resources remain part of the private implementation.

---

# Related Documentation

- `../../docs/event-model.md`
- `../../docs/hive-mind.md`
- `../../docs/infrastructure.md`
- `../docker-compose.yml`