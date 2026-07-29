<p align="center">
  <img src="../../assets/icons/phantom-dns.png" width="120" alt="Phantom DNS">
</p>

# Phantom DNS

## Purpose

This document describes Phantom DNS, a Phantom Ecosystem service focused on DNS reconnaissance and subdomain takeover detection.

Phantom DNS generates candidate subdomains for configured targets, probes the resulting endpoints asynchronously, and identifies signatures associated with abandoned cloud services.

---

## Overview

Phantom DNS is the subdomain takeover detection service of the Phantom Ecosystem.

It combines a large target-domain list with a prefix dictionary, performs high-volume asynchronous checks, inspects HTTP responses for known orphaned-service signatures, and reports candidates that require responsible manual validation.

---

## Responsibilities

The module is designed to:

- Generate candidate subdomains from target domains and prefixes
- Perform asynchronous DNS and HTTP checks
- Inspect responses for abandoned service signatures
- Control request bursts with chunking and delays
- Report potential takeover conditions independently

---

## Scope

The module operates within the following scope:

- Configured target domains
- Configured subdomain prefixes
- DNS resolution results
- HTTP response bodies and status information
- Known orphaned-service signatures

It is **not** responsible for:

- Claiming or registering third-party resources
- Automated exploitation
- General vulnerability scanning outside DNS takeover indicators
- Hive event publication or task consumption
- Centralized result persistence

---

## Architecture Role

**Domain:** DNS reconnaissance and subdomain takeover detection  
**Hive role:** None

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `dns_monitor.py` | Generates candidates, performs asynchronous checks, matches cloud-service signatures, throttles request batches, and reports findings. |
| `dns_targets.json` | Stores target domains and the configured prefix set. |
| `Dockerfile` | Defines the isolated Python runtime. |

---

## Workflow

1. Load target domains and subdomain prefixes.
2. Generate the candidate endpoint set.
3. Resolve or request candidates asynchronously in controlled batches.
4. Inspect responses for supported abandoned-service signatures.
5. Report candidates to Discord for responsible validation.

---

## Inputs

- `dns_targets.json`
- DNS and HTTP responses
- Signature definitions
- Discord runtime configuration

---

## Outputs

- Discord alerts containing the candidate hostname and matched service signature
- Transient request and response data only

---

## Hive Mind Participation

Current role:

- Producer: No
- Consumer: No
- Coordinator: No

Phantom DNS does not currently participate in the Hive Mind. It performs no active Redis publication or delegated task consumption.

---

## Communication

- Outbound DNS and HTTP requests
- Outbound Discord webhook requests
- Local target configuration
- No active Redis Pub/Sub communication

---

## Dependencies

- Python 3
- `aiohttp`
- `asyncio`
- `BeautifulSoup4` when HTML parsing is required
- Docker

---

## Configuration and State

- `dns_targets.json` contains target domains and prefixes.
- The current reference deployment does not require a centralized DNS result database.
- Runtime request state is local to the process.
- Redis is not used for persistence.

---

## Runtime Characteristics

- Independent Docker container
- High-volume asynchronous network workload
- Batching and delay controls
- Direct Discord reporting
- Shared Contabo VPS deployment

---

## Failure Behavior

- DNS failures, timeouts, and unavailable hosts are expected and should be isolated per candidate.
- Rate limits or WAF behavior can reduce coverage for a scan cycle.
- Module downtime does not affect other services or Hive chains.
- No Redis replay is involved.

---

## Security Considerations

- Only authorized target domains should be included.
- Potential takeover conditions must be manually validated and handled through responsible disclosure.
- The service must not automatically claim external resources.
- Private target lists and operational webhooks remain outside the public repository.

---

## Current Limitations

- Signature matching can produce false positives or become outdated as providers change responses.
- High-volume enumeration is constrained by network, DNS, and target rate limits.
- The module does not participate in Redis correlation.
- The public repository excludes the live target and signature inventories.

---

## Summary

Phantom DNS autonomously generates and probes candidate subdomains, identifies response signatures associated with abandoned cloud services, and reports potential takeover conditions to Discord for responsible validation.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)
