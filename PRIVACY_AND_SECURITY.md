# Privacy, Security & Zero-Egress Architecture

**Product**: CAB Quorum e-Sign & Rovo Release Ledger for Jira  
**App Key**: `com.evidencepair.jira.cab.quorum.ledger`  
**Vendor**: EvidencePair LLC  
**Security Contact**: `support@titlecovenant.com`

---

## 1. 100% Runs on Atlassian (Zero External Egress)
- **Runtime**: Atlassian Forge `nodejs24.x` (`arm64`, `256 MB`).
- **Zero External Remotes**: This application declares **zero** `remotes:` endpoints and transmits **zero** data outside the customer's Atlassian Cloud tenant.
- **Zero External Databases**: This application does not use Forge SQL, Forge KVS, or third-party cloud storage.

---

## 2. Host-Native Data Storage (`<32 KB` Jira Entity Properties)
All governance policies and cryptographic e-Signature ledgers are stored exclusively inside the customer's Jira Cloud instance using native Jira Entity Properties:
1. **Project Governance Policy (`cabQuorumPolicy`)**:
   - Stored at `/rest/api/3/project/{projectId}/properties/cabQuorumPolicy` (`<32 KB` enforced via `assertPropertyPayloadUnder32KB`).
2. **Issue Merkle Quorum Ledger (`cabQuorumLedger`)**:
   - Stored at `/rest/api/3/issue/{issueKey}/properties/cabQuorumLedger` (`<32 KB` enforced via `assertPropertyPayloadUnder32KB`).
   - Stores only Atlassian `accountId`, role title, Meaning of Signature attestation, ISO-8601 timestamp, SHA-256 Payload Fingerprint, and chained SHA-256 Merkle hashes.

---

## 3. Least-Privilege Scopes
The application declares strictly two GA Jira scopes in `manifest.yml`:
- `read:jira-work`: Reads issue scope fields (`summary`, `description`, `priority`, `subtasks`, `issuelinks`) and host Entity Properties to compute payload fingerprints and verify Merkle chains.
- `write:jira-work`: Writes cryptographic signature chains to host Entity Properties and applies compliance status labels (`rovo-cab-verified`, `cab-drift-quarantined`).

---

## 4. Data Retention, Deletion & Residency
Because all application state resides in native Jira Project and Issue Entity Properties:
- **Data Residency**: Automatically inherits the customer's Jira Cloud data residency region.
- **Data Deletion**: Deleting a Jira issue or project immediately and permanently deletes its associated `cabQuorumLedger` and `cabQuorumPolicy` properties.

---

## 5. Vulnerability Disclosure & Support SLA
Report any security or privacy inquiries to `support@titlecovenant.com`. EvidencePair LLC acknowledges all security reports within 24 hours.
