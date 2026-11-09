# CAB Quorum e-Sign & Rovo Release Ledger for Jira

**Enterprise Sequential 4-Role Merkle-Chained SHA-256 Quorum Matrix, 21 CFR Part 11 Payload-Drift Tamper Detection, Pre-Flight Release Readiness Audit, Workflow Transition Gate & Cryptographic Compliance Certificate Exporter for Jira & Jira Service Management**

- **Vendor**: EvidencePair LLC (support: <https://evidencepair.com/support/>, support@evidencepair.com)
- **App ID**: `ari:cloud:ecosystem::app/48e50169-f4f6-49d5-8bca-b13911169555`
- **App Key**: `com.evidencepair.jira.cab.quorum.ledger`
- **Runtime**: Atlassian Forge `nodejs24.x` (no external egress, no Forge SQL or KVS)
- **Supported Products**: Jira Cloud & Jira Service Management Cloud

---

## Key Enterprise Capabilities

1. **Sequential 4-Role CAB Merkle-Chained SHA-256 Quorum Matrix (`cab-quorum-issue-panel`)**
   - Enforces strict sequential Change Advisory Board (CAB) role sign-offs:
     - **Stage 1**: `Engineering Lead — Code & Rollback Verified`
     - **Stage 2**: `QA & Automated Test Lead — Regression Passed`
     - **Stage 3**: `Security & Compliance Officer — SAST/SCA & Zero PII Leak`
     - **Stage 4**: `CAB Chair — Production Release Authorized`
   - Every stage requires a mandatory **21 CFR Part 11 Meaning of Signature** attestation and chains cryptographically to the previous stage's SHA-256 hash (`Genesis "0".repeat(64) -> Stage 1 -> Stage 2 -> Stage 3 -> Stage 4`).

2. **Real-Time Payload-Drift Tamper Detection (21 CFR Part 11 §11.70 Record Linking)**
   - Computes a deterministic **Issue Payload Fingerprint**:
     `SHA256(issueKey + "|" + summary + "|" + descriptionDigest + "|" + priority)`
   - Recomputes the entire Merkle chain on every issue load and workflow transition. If ticket scope (`summary`, `description`, or `priority`) is altered after any CAB signature, the ledger immediately flags `BLOCKED_PAYLOAD_DRIFT_DETECTED` and quarantines the release.

3. **Pre-Flight Release Readiness Audit**
   - The panel's **Run Pre-Flight Audit** button runs a fixed set of rule checks. It makes no AI or Rovo call and sends no data anywhere.
   - Executes a deterministic 6-point pre-flight release readiness audit across ticket scope completeness, rollback runbook verification, child subtask completion, linked open blockers, zero payload drift, and sequential CAB quorum progress (`0–100%` Readiness Score).
   - Applies the Jira issue label `rovo-cab-verified` or `cab-drift-quarantined`, as the person who runs it.
   - Signing, running the audit and resetting the ledger need permission to edit the issue. Each signature records the account ID of the person who signed.

4. **Jira Workflow Validator & 21 CFR Part 11 Compliance Certificate Exporter (`cab-quorum-release-validator`, `cab-quorum-project-ledger`, `cab-quorum-global-admin`)**
   - Blocks production release/resolution transitions until all 4 CAB roles have signed in sequence with zero payload drift (`CLEARED_MERKLE_QUORUM_SEALED`).
   - The project page lists the project's issues that have a CAB ledger, and exports verifiable **21 CFR Part 11 Cryptographic Release Certificates** and formula-neutralized **RFC-4180 CSV Release Ledgers**.
   - Each project uses the CAB policy saved on it, or the default FDA 21 CFR Part 11 policy. This version cannot change a project's policy.

---

## Documentation Index
- [User & Administrator Guide](./USER_GUIDE.md)
- [Privacy Policy](./PRIVACY_POLICY.md)
- [Security](./SECURITY.md)
- [Privacy, Security & Zero-Egress Architecture](./PRIVACY_AND_SECURITY.md)
- [End-User License Agreement (EULA)](./EULA.md)
- [Marketplace Listing Copy](./LISTING_COPY.md)
- [Enterprise Cloud Pricing Table](./PRICING_TABLE.md)
