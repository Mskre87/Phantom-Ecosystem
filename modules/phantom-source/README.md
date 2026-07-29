<p align="center">
  <img src="../../assets/icons/phantom-source.png" width="160" alt="Phantom Source">
</p>

<h1 align="center">Phantom Source</h1>

<p align="center">
GitHub global event monitor and source-code patch secret scanner.
</p>

<p align="center">

![Status](https://img.shields.io/badge/status-production-2EA043)
![Docker](https://img.shields.io/badge/docker-supported-2496ED)
![Architecture](https://img.shields.io/badge/architecture-event--driven-F79009)
![Hive](https://img.shields.io/badge/hive-producer-7B42F6)

</p>

---

## 📖 Overview

Phantom Source continuously inspects relevant GitHub push activity, scans commit patches for exposed secrets, suppresses repeated commit alerts with a bounded in-memory cache, reports findings to Discord, and publishes selected intelligence to the Hive Mind as a producer.

---

## 🚀 Status

**Production**

Phantom Source is part of the active single-VPS Docker Compose deployment.

---

## 🎯 Responsibilities

- GitHub global event monitoring
- Commit patch retrieval
- Source-code secret scanning
- LRU-based duplicate suppression
- Hive event publication
- Direct Discord alerting

---

## 📂 Repository Structure

This public documentation entry contains no implementation source code.

```text
modules/phantom-source/
├── README.md
└── CHANGELOG.md
```

The operational implementation is maintained separately in a private repository.

---

## 📚 Documentation

- [Architecture](../../docs/architecture.md)
- [Infrastructure](../../docs/infrastructure.md)
- [Deployment](../../docs/deployment.md)
- [Module documentation](../../docs/modules/phantom-source.md)
- [Hive Mind](../../docs/hive-mind.md)
- [Event Model](../../docs/event-model.md)

---

## 🛣️ Roadmap

Future architectural changes are tracked in the central [Phantom Ecosystem Roadmap](../../ROADMAP.md).

---

## 📄 License

The public documentation is distributed under the Apache License 2.0.

Refer to the [LICENSE](../../LICENSE) file for the complete terms.
