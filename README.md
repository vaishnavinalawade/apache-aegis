# apache-aegis
# 🛡️ Apache Aegis (Incubating)
**Dual-Validation Security Framework for Binary and Configuration Integrity Verification**

---

## 🔍 Overview

**Apache Aegis** is an open-source framework that ensures *no executable, configuration, or binary* is changed, deployed, or executed without two independent approvals — one automated and one human or policy-based.  

Aegis establishes a **cryptographically verifiable chain of trust** around system mutations, protecting against malware, trojans, botnets, supply-chain tampering, and privilege-escalation attacks.  

It extends the governance model of **Apache Ranger** and the attestation philosophy of **Sigstore**, combining both into a real-time prevention and audit framework for enterprises and open systems.

---

## 🧱 Key Features

| Capability | Description |
|-------------|-------------|
| **Dual Validation Workflow** | Every change is verified twice — automated hash/signature validation and independent human or quorum approval. |
| **Immutable Audit Ledger** | Each event is hashed and written to a Merkle-tree ledger for tamper-proof auditing. |
| **Pluggable Policy Engine** | Uses [Open Policy Agent](https://www.openpolicyagent.org/) (Rego) for granular approval and enforcement rules. |
| **Cross-Platform Agent** | File-system and binary watcher for Linux, Kubernetes, and Android. |
| **CI/CD Integration** | Hooks into GitHub Actions, Jenkins, or GitLab CI to validate artifacts pre-deployment. |
| **Attack-aware Controls** | Built-in mitigation logic for malware, worms, ransomware, and credential-dumping tools. |
| **Transparency-ready Ledger** | Supports distributed attestation and external audit verification. |

---

## ⚙️ Architecture
                          ┌──────────────────────┐
                          │   Aegis REST API     │
                          │   (Control Plane)    │
                          └─────────┬────────────┘
                                    │
                                    │  (mTLS)
                                    ▼
                       ┌────────────┴────────────┐
                       │     Policy Engine       │
                       │       (OPA / Rego)      │
                       └────────────┬────────────┘
                                    │
                        (Signed Approval Tokens)
                                    │
                                    ▼
                 ┌──────────────────┴──────────────────┐
                 │       Local Agent / Daemon          │
                 │     (eBPF / fanotify hooks)         │
                 └──────────────────┬──────────────────┘
                                    │
                        (Immutable Ledger Writes)
                                    │
                                    ▼
                          ┌─────────┴─────────┐
                          │     Audit Log     │
                          │    (Merkle Tree)  │
                          └───────────────────┘


---

## 🧩 Project Modules

| Module | Purpose | Language |
|---------|----------|----------|
| **aegis-api** | REST API for change requests, approvals, and audit queries | Python / FastAPI |
| **aegis-agent** | Host daemon watching file & binary modifications | Go / Rust |
| **aegis-ledger** | Merkle-tree / blockchain-style immutable ledger | Go |
| **aegis-policy** | Policy rules & validation engine (OPA/Rego) | Go / Rego |
| **aegis-cli** | Command-line utility for admins and validators | Python |
| **aegis-android** | Android proof-of-concept for package verification | Kotlin / Java |
| **docs** | Design docs, whitepapers, and governance materials | Markdown |

---

## 🔐 Security Model

- **mTLS Everywhere** – every inter-service call uses mutual TLS.  
- **Short-lived Approval Tokens** – signed JWTs, verified by HSM/KMS.  
- **Role-based Access Control (RBAC)** – granular approval scopes.  
- **Hardware-backed MFA** for human approvers.  
- **Immutable Ledger Storage** for forensic traceability.  
- **Automatic Lockdown** when anomalous mutation patterns occur.  

---

## 🧠 Attack Coverage and Dual-Validation Effectiveness

| Attack Vector | Example Behavior | Dual-Validation Impact | Residual Risk |
|----------------|------------------|------------------------|----------------|
| **Worms / Ransomware** | Self-propagating binary replacement | 🟢 Blocks unauthorized file writes, quarantines modified binaries | Fileless variants need memory defense |
| **Trojans / Malware** | Disguised malicious binaries | 🟢 Signature mismatch / provenance failure halts install | Compromised signer requires external attestation |
| **Botnets** | Remote C2 agent installation | 🟢 Prevents persistent service registration | Existing process abuse may bypass |
| **Zero-Day Exploits** | Memory corruption & privilege escalation | 🟠 Detects post-exploit persistence attempts | Kernel-level memory exploit may occur first |
| **DNS Tunneling** | Hidden data exfiltration via DNS | 🟠 Detects installation of tunneling binaries | Legitimate tool misuse requires network IDS |
| **Mimikatz / Dumpers** | Credential theft utilities | 🟢 Blocks unapproved dumpers, logs anomalies | In-memory LSASS access needs EDR |
| **Supply-Chain Compromise** | Tampered build artifacts | 🟢 Enforces dual-signed CI/CD provenance | Must integrate with Sigstore / SLSA |
| **Spyware / Pegasus** | Zero-click implants & kernel hooks | 🟠 Detects persistence, binary hooks | Requires Secure Boot + attestation |

---

## 🚀 Quick Start (Developer Mode)

```bash
# Clone repository
git clone https://github.com/<your-org>/apache-aegis.git
cd apache-aegis

# Start API
cd aegis-api
pip install -r requirements.txt
uvicorn aegis_api.app:app --reload

# Start Agent
cd ../aegis-agent
go run main.go


