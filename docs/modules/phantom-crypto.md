<p align="center">
  <img src="../../assets/icons/phantom-crypto.png" width="120" alt="Phantom Crypto">
</p>

# Phantom Crypto

## Purpose

This document describes Phantom Crypto, a Phantom Ecosystem service focused on smart-contract and Web3 security analysis.

Phantom Crypto monitors selected DeFi repositories, inspects modified Solidity and Vyper contracts, applies heuristic security checks, and runs Slither static analysis.

---

## Overview

Phantom Crypto is the smart-contract auditing service of the Phantom Ecosystem.

It monitors configured blockchain protocol repositories, retrieves changed `.sol` and `.vy` files, applies targeted heuristic checks, invokes Slither for static analysis, reports relevant findings, and removes temporary contract files after processing.

---

## Responsibilities

The module is designed to:

- Monitor configured DeFi repositories
- Retrieve changed Solidity and Vyper contracts
- Apply heuristic checks for high-impact contract patterns
- Run Slither static analysis
- Report findings independently
- Delete temporary analysis material after processing

---

## Scope

The module operates within the following scope:

- Public smart-contract repository changes
- Solidity and Vyper source files
- Configured DeFi targets
- Slither analysis output

It is **not** responsible for:

- On-chain transaction execution
- Wallet management
- Automated exploitation
- Hive event publication or task consumption
- Long-term contract source retention

---

## Architecture Role

**Domain:** Smart contract and Web3 security analysis  
**Hive role:** None

The module runs as an independent Docker service within the single-host Phantom Ecosystem deployment. Its domain workflow remains isolated from the private implementation of the other modules.

---

## Internal Components

| Component | Responsibility |
|-----------|----------------|
| `crypto_monitor.py` | Polls configured repositories, retrieves contract changes, runs heuristic checks, invokes Slither, reports findings, and cleans temporary files. |
| `crypto_targets.json` | Stores the monitored DeFi repository targets. |
| `Dockerfile` | Defines the Python and Slither execution environment. |

---

## Workflow

1. Poll the configured protocol repositories.
2. Identify changed `.sol` or `.vy` files.
3. Download each selected contract to a temporary analysis area.
4. Apply heuristic checks for unsafe access control, destructive operations, and external call patterns.
5. Run Slither static analysis.
6. Report relevant findings to Discord.
7. Delete temporary contract files.

---

## Inputs

- Repository API responses and changed contract files
- `crypto_targets.json`
- Runtime API and Discord configuration
- Local monitoring state when configured

---

## Outputs

- Discord alerts and analysis summaries
- Temporary contract files that are deleted after analysis
- Updated lightweight monitoring state

---

## Hive Mind Participation

Current role:

- Producer: No
- Consumer: No
- Coordinator: No

Phantom Crypto does not currently participate in the Hive Mind. It reports findings independently and performs no active Redis Pub/Sub communication.

---

## Communication

- Outbound repository API requests
- Local Slither subprocess execution
- Outbound Discord webhook requests
- No active Redis Pub/Sub communication

---

## Dependencies

- Python 3
- `aiohttp`
- `asyncio`
- Slither
- Solidity analysis toolchain
- Docker

---

## Configuration and State

- `crypto_targets.json` contains monitored repositories.
- The deployment can mount lightweight local state to avoid repeated processing.
- Temporary contracts are removed after analysis.
- Redis is not used for module state.

---

## Runtime Characteristics

- Independent Docker container
- Asynchronous repository monitoring
- Local static-analysis subprocesses
- Temporary disk usage
- Direct Discord reporting

---

## Failure Behavior

- Repository or API failures pause only the affected target.
- Slither failure should not prevent the monitor from continuing with later contracts.
- Module downtime does not affect other Phantom services or Hive chains.
- Temporary data should be removed during normal cleanup and container recreation.

---

## Security Considerations

- Only public and authorized repository content should be analyzed.
- Private API and webhook credentials remain outside the public repository.
- Temporary contract content is not intended for long-term retention.
- The public documentation omits target inventories and full detection heuristics.

---

## Current Limitations

- Static analysis can produce findings that require manual validation.
- Toolchain compatibility can vary across compiler and contract versions.
- The module does not publish or consume Hive events.
- The public repository excludes source code, target lists, credentials, and operational findings.

---

## Summary

Phantom Crypto autonomously monitors smart-contract repositories, applies heuristic checks, runs Slither, reports relevant results to Discord, and removes temporary analysis files after processing.

---

## Related Documentation

- [`../architecture.md`](../architecture.md)
- [`../deployment.md`](../deployment.md)
- [`../infrastructure.md`](../infrastructure.md)
- [`../modules.md`](../modules.md)
- [`../security-model.md`](../security-model.md)
- [`../hive-mind.md`](../hive-mind.md)
- [`../event-model.md`](../event-model.md)
