<p align="center">
  <img src="../../assets/icons/phantom-correlation.png" width="120" alt="Phantom Correlation">
</p>

# Phantom Correlation

## Purpose

This document describes Phantom Correlation, a Phantom Ecosystem service focused on event correlation and task delegation.

Phantom Correlation consumes supported Hive intelligence, evaluates implemented correlation rules, notifies the dedicated Hive Mind Discord channel, and publishes one-way delegated tasks.

---

## Overview

Phantom Correlation is the stateless coordination service of the Hive Mind.

It runs an asynchronous Redis Pub/Sub listener and an internal FastAPI ingress endpoint. Supported events are evaluated against implemented rules and translated into delegated tasks for specialized consumers.

The engine does not execute the delegated analysis, persist events, manage workflow state, wait for results, or retry missed tasks.

---

## Responsibilities

The module is designed to:

- Subscribe to `phantom_hive`
- Validate and evaluate supported normalized events
- Apply the implemented Source-to-Supply and JS-to-GraphQL rules
- Notify the dedicated Hive Mind Discord channel
- Publish tasks to `phantom_supply_tasks` and `phantom_graphql_tasks`
- Expose the internal `/api/v1/ingest` compatibility endpoint
- Remain stateless and independent from consumer execution

---

## Scope

The module operates within the following scope:

- Normalized events from `phantom_hive`
- Internal HTTP payloads accepted by `/api/v1/ingest`
- `X-Hive-Secret` authentication
- Implemented correlation rules

It is **not** responsible for:

- Performing security analysis
- Persisting event or task history
- Deduplicating producer events
- Tracking task status or completion
- Receiving consumer results
- Providing delivery guarantees or retries

---

## Architecture Role

**Domain:** Event correlation and task delegation  
**Hive role:** Coordinator

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `correlation_engine.py` | Runs the Redis listener and FastAPI ingress, validates events, applies correlation rules, sends Hive notifications, and publishes delegated tasks. |
| `Dockerfile` / `requirements.txt` | Define the Python, Redis client, FastAPI, and Uvicorn runtime. |

---

## Workflow

1. Receive a supported event from `phantom_hive` or the internal ingress endpoint.
2. Validate the payload and identify its event type.
3. Evaluate the implemented correlation rule.
4. Send a notification to the Hive Mind Discord channel.
5. Publish a task to the corresponding consumer channel.
6. Return immediately without waiting for the consumer.

---

## Inputs

- Redis events from `phantom_hive`
- Internal HTTP POST requests to `/api/v1/ingest`
- `X-Hive-Secret`
- Runtime Redis and Discord configuration

---

## Outputs

- Hive Mind Discord notifications
- Tasks published to `phantom_supply_tasks`
- Tasks published to `phantom_graphql_tasks`
- No persisted event or result records

---

## Hive Mind Participation

Current role:

- Producer: No
- Consumer: No
- Coordinator: Yes

Phantom Correlation is the Hive coordinator. It subscribes to the shared ingress channel and publishes delegated tasks, but it is not a hunting producer or delegated consumer.

---

## Communication

- Redis subscription to `phantom_hive`
- Redis publication to module-specific task channels
- Internal FastAPI on port `8080` without a public port mapping
- Outbound Hive Mind Discord webhook requests

---

## Dependencies

- Python 3
- Redis client
- FastAPI
- Uvicorn
- Docker

---

## Configuration and State

- `HIVE_SECRET_KEY` and operational webhook values are supplied through runtime configuration.
- The engine uses only volatile Python memory while processing an event.
- It has no database, JSON history, Redis-backed state, queue, or result store.
- Restarting the container clears all internal execution context.

---

## Runtime Characteristics

- Independent lightweight Docker container
- Concurrent Redis listener and FastAPI server
- Internal-only port `8080`
- Stateless execution
- Fire-and-forget task publication

---

## Failure Behavior

- Events published while Correlation is offline are lost.
- Tasks published while a consumer is offline are lost.
- No acknowledgement, retry, replay, or recovery mechanism exists.
- Consumer failures are not detected because results do not return to the engine.

---

## Security Considerations

- The FastAPI ingress is protected by `X-Hive-Secret` and remains internal to `phantom_net`.
- Redis and FastAPI ports are not publicly published.
- Operational secrets remain outside the public repository.
- Payloads should contain only the information required for delegated validation.

---

## Current Limitations

- Only Phantom Source and Phantom JS currently publish supported events.
- Only Phantom Supply and Phantom GraphQL currently consume delegated tasks.
- The engine has no persistent history, deduplication, acknowledgements, retries, or result aggregation.
- The ingress endpoint is retained for internal compatibility and future external agents but is not publicly exposed.

---

## Summary

Phantom Correlation is a stateless, one-way correlation and delegation engine. It consumes supported Hive events, matches implemented rules, notifies Discord, and publishes ephemeral tasks without managing workflows or tracking consumer results.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)
