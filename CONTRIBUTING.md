# Contributing to Apache Aegis (Incubating)

Welcome to **Apache Aegis** — a Dual-Validation Security Framework for Binary and Configuration Integrity.  
This document outlines contribution workflows, module ownership, and coding standards consistent with the **Apache Incubator** guidelines.

---

## 🧭 Project Philosophy

Apache Aegis aims to establish a **cryptographically verifiable chain of trust** for all code and configuration changes across platforms.  
We believe in:
- Open collaboration and transparent governance.
- Security-driven design with verifiable auditability.
- Modular, language-agnostic architecture.

---

## 🧱 Project Modules & Ownership

| Module | Description | Language | Current Owner / Maintainer |
|---------|--------------|-----------|-----------------------------|
| **aegis-api** | REST control plane for change requests, approvals, and audit logs. | Python / FastAPI | Core Team |
| **aegis-agent** | Local daemon for file monitoring and enforcement (hooks into eBPF / fanotify). | Go / Rust | Security Subteam |
| **aegis-ledger** | Immutable Merkle-tree / blockchain-based audit ledger. | Go / Rust | Ledger Subteam |
| **aegis-policy** | Policy engine integrating with OPA / Rego for granular rules. | Go / Rego | Policy Subteam |
| **aegis-cli** | Command-line client for admins and validators. | Python | CLI Subteam |
| **aegis-android** | Android integrity proof-of-concept (APK verification and runtime attestation). | Kotlin / Java | Mobile Subteam |
| **docs** | Design documents, threat models, compliance and incubation paperwork. | Markdown | Documentation Subteam |
| **tests** | Integration, fuzzing, and regression tests. | Mixed | QA Subteam |

If you'd like to become a module maintainer, file an issue or PR proposing ownership with your experience summary.

---

## ⚙️ Development Workflow

1. **Fork the repository**
   ```bash
   git clone https://github.com/vaishnavinalawade/apache-aegis.git
   git checkout -b feature/
