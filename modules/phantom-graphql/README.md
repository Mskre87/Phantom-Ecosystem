<p align="center">
  <img src="../../assets/icons/phantom-graphql.png" width="160" alt="Phantom GraphQL">
</p>

<h1 align="center">Phantom GraphQL</h1>

<p align="center">
Scheduled and delegated GraphQL introspection analysis service.
</p>

<p align="center">

![Status](https://img.shields.io/badge/status-production-2EA043)
![Docker](https://img.shields.io/badge/docker-supported-2496ED)
![Architecture](https://img.shields.io/badge/architecture-event--driven-F79009)
![Hive](https://img.shields.io/badge/hive-consumer-7B42F6)

</p>

---

## 📖 Overview

Phantom GraphQL combines a controlled daily introspection scan with delegated token-assisted tasks from the Hive Mind. It consumes `phantom_graphql_tasks`, reports findings directly to Discord, and returns no completion event to Phantom Correlation.

---

## 🚀 Status

**Production**

Phantom GraphQL is part of the active single-VPS Docker Compose deployment.

---

## 🎯 Responsibilities

- Daily endpoint introspection
- Schema metadata analysis
- Delegated token-assisted checks
- `phantom_graphql_tasks` consumption
- Direct Discord reporting

---

## 📂 Repository Structure

This public documentation entry contains no implementation source code.

```text
modules/phantom-graphql/
├── README.md
└── CHANGELOG.md
```

The operational implementation is maintained separately in a private repository.

---

## 📚 Documentation

- [Architecture](../../docs/architecture.md)
- [Infrastructure](../../docs/infrastructure.md)
- [Deployment](../../docs/deployment.md)
- [Module documentation](../../docs/modules/phantom-graphql.md)
- [Hive Mind](../../docs/hive-mind.md)
- [Event Model](../../docs/event-model.md)

---

## 🛣️ Roadmap

Future architectural changes are tracked in the central [Phantom Ecosystem Roadmap](../../ROADMAP.md).

---

## 📄 License

The public documentation is distributed under the Apache License 2.0.

Refer to the [LICENSE](../../LICENSE) file for the complete terms.
