<p align="center">
  <img src="../../assets/icons/phantom-graphql.png" width="120" alt="Phantom GraphQL">
</p>

# Phantom GraphQL

## Purpose

This document describes Phantom GraphQL, a Phantom Ecosystem service focused on GraphQL endpoint and schema exposure validation.

Phantom GraphQL probes configured GraphQL endpoints on a controlled schedule and also processes delegated token-assisted introspection tasks from the Hive Mind.

---

## Overview

Phantom GraphQL is the GraphQL introspection analysis service of the Phantom Ecosystem.

Its autonomous mode sends a limited introspection request to common GraphQL paths for configured targets approximately once every 24 hours.

Its delegated mode consumes tasks created from supported Phantom JS findings, applies supplied authorization context when present, analyzes the returned schema information, and reports results directly to Discord.

---

## Responsibilities

The module is designed to:

- Probe configured GraphQL endpoints on a daily schedule
- Send supported introspection queries
- Parse schema, type, and mutation metadata
- Consume delegated tasks from `phantom_graphql_tasks`
- Use delegated authorization context when supplied
- Report findings independently without returning results to Correlation

---

## Scope

The module operates within the following scope:

- Configured GraphQL target endpoints
- HTTP JSON responses
- Delegated tasks containing endpoint and optional authorization context
- Common GraphQL paths such as `/graphql` and `/api/graphql`

It is **not** responsible for:

- Database access
- Executing destructive mutations
- Publishing results to `phantom_hive`
- Tracking delegated task completion centrally
- General API fuzzing outside the documented introspection workflow

---

## Architecture Role

**Domain:** GraphQL endpoint and schema exposure validation  
**Hive role:** Consumer

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `graphql_monitor.py` | Runs scheduled endpoint checks, sends introspection requests, parses responses, consumes delegated GraphQL tasks, and reports findings. |
| `graphql_targets.json` | Stores the endpoints used by the autonomous daily scan. |
| `Dockerfile` | Defines the isolated Python runtime. |

---

## Workflow

1. Run the scheduled target scan or receive a delegated task.
2. Select a supported GraphQL endpoint path.
3. Send the introspection request with optional delegated authorization context.
4. Parse the JSON response and summarize exposed schema information.
5. Report the result directly to Discord.
6. Do not send a completion event back to Phantom Correlation.

---

## Inputs

- `graphql_targets.json`
- GraphQL endpoint responses
- Redis tasks from `phantom_graphql_tasks`
- Optional bearer-token context produced by Phantom JS
- Runtime Discord configuration

---

## Outputs

- Discord alerts and schema-exposure summaries
- No result event returned to Phantom Correlation
- No centralized schema archive

---

## Hive Mind Participation

Current role:

- Producer: No
- Consumer: Yes
- Coordinator: No

Phantom GraphQL participates as a consumer. It subscribes to `phantom_graphql_tasks`, does not publish to `phantom_hive`, and reports its own results directly to Discord.

---

## Communication

- Outbound GraphQL HTTP POST requests
- Redis subscription to `phantom_graphql_tasks`
- Outbound Discord webhook requests
- No direct calls to Phantom JS or Phantom Correlation

---

## Dependencies

- Python 3
- `aiohttp`
- Redis client integration
- Docker

---

## Configuration and State

- `graphql_targets.json` contains autonomous scan targets.
- The scheduled interval is approximately 24 hours.
- Delegated tasks are ephemeral and are not persisted by Redis.
- The module maintains no centralized execution history.

---

## Runtime Characteristics

- Independent Docker container
- Scheduled autonomous execution
- Event-triggered delegated execution
- Redis consumer
- Direct Discord reporting

---

## Failure Behavior

- Scheduled checks resume on the next normal cycle after transient endpoint failures.
- Tasks published while GraphQL is offline are lost because Redis Pub/Sub has no backlog.
- Malformed schema responses should not stop later targets.
- Phantom Correlation does not receive task status or results.

---

## Security Considerations

- Only authorized endpoints should be tested.
- Delegated authorization values must remain in volatile task payloads and must not be written to public documentation.
- The service should avoid destructive operations and remain limited to the documented validation workflow.
- Operational targets, tokens, and webhooks remain private.

---

## Current Limitations

- Open introspection does not by itself prove unauthorized data access.
- Delegated tasks have no acknowledgement, retry, or replay.
- The module does not return results to Correlation.
- The public repository excludes live endpoints, tokens, source code, and operational findings.

---

## Summary

Phantom GraphQL combines a controlled daily introspection scan with delegated token-assisted tasks from the Hive Mind. It consumes `phantom_graphql_tasks`, reports findings directly to Discord, and returns no completion event to Phantom Correlation.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)
