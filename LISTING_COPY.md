# Marketplace Listing Copy — CAB Quorum e-Sign & Rovo Release Ledger for Jira

- **App Name (48 chars)**: `CAB Quorum e-Sign & Rovo Release Ledger for Jira`
- **App Key**: `com.evidencepair.jira.cab.quorum.ledger`
- **Tagline / Summary**: Sequential 4-role Merkle SHA-256 CAB e-Signatures, 21 CFR Part 11 payload-drift tamper lock, Rovo readiness audit & workflow gate.
- **Categories**: Change Management, Security & Compliance, Workflow & Automation, IT & Helpdesk

---

## Highlights

### Highlight 1: Sequential 4-Role Merkle-Chained SHA-256 CAB Quorum Matrix
Enforce ordered electronic signatures across Engineering Lead, QA Lead, Security Officer, and CAB Chair. Every signature requires a 21 CFR Part 11 Meaning of Signature attestation and chains cryptographically from Genesis (`0000...`) to a tamper-evident Merkle Root Seal.

### Highlight 2: Real-Time 21 CFR Part 11 Payload-Drift Tamper Detection & Workflow Gate
Computes a live SHA-256 fingerprint over issue summary, description digest, and priority. If anyone alters ticket scope after a CAB member signs off, the workflow validator immediately blocks release transitions (`BLOCKED_PAYLOAD_DRIFT_DETECTED`) and quarantines the issue.

### Highlight 3: Rovo AI Pre-Flight Release Auditor & 21 CFR Part 11 Certificate Exporter
Run a 6-point pre-flight audit checking scope completeness, rollback runbooks, child subtasks, open blockers, payload drift, and Merkle quorum progress. Export verifiable 21 CFR Part 11 Cryptographic Release Certificates and formula-safe RFC-4180 CSV ledgers in 1 click.
