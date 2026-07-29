<p align="center">
  <img src="../../assets/icons/phantom-correlation.png" width="160" alt="Phantom Correlation">
</p>

<h1 align="center">Phantom Correlation</h1>

<p align="center">
Stateless Redis Pub/Sub correlation engine and one-way delegated task publisher.
</p>

<p align="center">

![Status](https://img.shields.io/badge/status-production-2EA043)
![Docker](https://img.shields.io/badge/docker-supported-2496ED)
![Architecture](https://img.shields.io/badge/architecture-event--driven-F79009)
![Hive](https://img.shields.io/badge/hive-coordinator-7B42F6)

</p>

---

## 📖 Overview

Phantom Correlation is a stateless, one-way correlation and delegation engine. It consumes supported Hive events, matches implemented rules, notifies Discord, and publishes ephemeral tasks without managing workflows or tracking consumer results.

---

## 🚀 Status

**Production**

Phantom Correlation is part of the active single-VPS Docker Compose deployment.

---

## 🎯 Responsibilities

- Hive event consumption
- Correlation rule evaluation
- One-way task delegation
- Hive Mind Discord notifications
- Internal FastAPI ingress
- Stateless execution

---

## 📂 Repository Structure

This public documentation entry contains no implementation source code.

```text
modules/phantom-correlation/
├── README.md
└── CHANGELOG.md
```

The operational implementation is maintained separately in a private repository.

---

## 📚 Documentation

- [Architecture](../../docs/architecture.md)
- [Infrastructure](../../docs/infrastructure.md)
- [Deployment](../../docs/deployment.md)
- [Module documentation](../../docs/modules/phantom-correlation.md)
- [Hive Mind](../../docs/hive-mind.md)
- [Event Model](../../docs/event-model.md)

---

## 🛣️ Roadmap

Future architectural changes are tracked in the central [Phantom Ecosystem Roadmap](../../ROADMAP.md).

---

## 📄 License

The public documentation is distributed under the Apache License 2.0.

Refer to the [LICENSE](../../LICENSE) file for the complete terms.
