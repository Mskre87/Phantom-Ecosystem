<p align="center">
  <img src="../../assets/icons/phantom-supply.png" width="160" alt="Phantom Supply">
</p>

<h1 align="center">Phantom Supply</h1>

<p align="center">
Dependency confusion discovery and validation service for package manifests and delegated package intelligence.
</p>

<p align="center">

![Status](https://img.shields.io/badge/status-production-2EA043)
![Docker](https://img.shields.io/badge/docker-supported-2496ED)
![Architecture](https://img.shields.io/badge/architecture-event--driven-F79009)
![Hive](https://img.shields.io/badge/hive-consumer-7B42F6)

</p>

---

## 📖 Overview

Phantom Supply autonomously inspects package manifests and also consumes delegated package tasks. It validates public registry availability, reports potential dependency confusion conditions directly to Discord, and does not return results to the stateless Correlation Engine.

---

## 🚀 Status

**Production**

Phantom Supply is part of the active single-VPS Docker Compose deployment.

---

## 🎯 Responsibilities

- Package manifest inspection
- NPM and PyPI availability checks
- Dependency confusion validation
- `phantom_supply_tasks` consumption
- Local state tracking
- Direct Discord reporting

---

## 📂 Repository Structure

This public documentation entry contains no implementation source code.

```text
modules/phantom-supply/
├── README.md
└── CHANGELOG.md
```

The operational implementation is maintained separately in a private repository.

---

## 📚 Documentation

- [Architecture](../../docs/architecture.md)
- [Infrastructure](../../docs/infrastructure.md)
- [Deployment](../../docs/deployment.md)
- [Module documentation](../../docs/modules/phantom-supply.md)
- [Hive Mind](../../docs/hive-mind.md)
- [Event Model](../../docs/event-model.md)

---

## 🛣️ Roadmap

Future architectural changes are tracked in the central [Phantom Ecosystem Roadmap](../../ROADMAP.md).

---

## 📄 License

The public documentation is distributed under the Apache License 2.0.

Refer to the [LICENSE](../../LICENSE) file for the complete terms.
