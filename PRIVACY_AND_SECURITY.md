# Privacy, Security & Zero-Egress Architecture

The app's privacy policy is [PRIVACY_POLICY.md](./PRIVACY_POLICY.md), and its security documentation is [SECURITY.md](./SECURITY.md). This page is a short technical summary of both.

**Product**: CAB Quorum e-Sign & Rovo Release Ledger for Jira  
**App Key**: `com.evidencepair.jira.cab.quorum.ledger`  
**Vendor**: EvidencePair LLC  
**Security Contact**: security@evidencepair.com  
**Support**: <https://evidencepair.com/support/> (support@evidencepair.com)

---

## 1. Atlassian Forge Only (No External Egress)
- **Runtime**: Atlassian Forge `nodejs24.x` (`arm64`, `256 MB`).
- **No External Remotes**: The manifest declares no `remotes:` endpoints and no external permissions. The app sends no data outside the customer's Atlassian site, and its readiness audit calls no Rovo or other AI service.
- **No External Databases**: The app does not use Forge SQL, Forge KVS, or third-party cloud storage.

---

## 2. Host-Native Data Storage (`<32 KB` Jira Entity Properties)
All of the app's own data is stored inside the customer's Jira Cloud site as two Jira entity properties. The app checks each one is under 32 KB before writing it (`assertPropertyPayloadUnder32KB`).
1. **Project Governance Policy (`cabQuorumPolicy`)**:
   - Stored at `/rest/api/3/project/{projectId}/properties/cabQuorumPolicy`.
   - The admin page writes it to the project with ID `10035` only, a fixed value from the app's built-in test configuration, and does not check Jira's reply. Projects without this property use the app's built-in default policy.
2. **Issue Merkle Quorum Ledger (`cabQuorumLedger`)**:
   - Stored at `/rest/api/3/issue/{issueKey}/properties/cabQuorumLedger`.
   - Stores, for each signed stage, the role, the Meaning of Signature text, an ISO-8601 timestamp, the SHA-256 Payload Fingerprint and the chained SHA-256 Merkle hashes.
   - Each signature also carries a signer account ID and name. In this version they are the same fixed values from the app's built-in test configuration for every signature, so the ledger does not record who signed.
   - Also stores a copy of the issue's summary and priority, a SHA-256 hash of its description, and the latest readiness audit result. The full list is in the [privacy policy](./PRIVACY_POLICY.md).

---

## 3. Scopes
The application declares two Jira scopes in `manifest.yml`:
- `read:jira-work`: Reads issue fields (`summary`, `description`, `priority`, `status`, `issuetype`, `subtasks`, `issuelinks`, `labels`) and the two entity properties, to compute payload fingerprints and verify Merkle chains.
- `write:jira-work`: Writes and deletes the `cabQuorumLedger` property, writes the `cabQuorumPolicy` property from the admin page, and applies the labels `rovo-cab-verified` and `cab-drift-quarantined`.

Every request to Jira runs as the app (`asApp`), so Jira applies the app's permissions whoever is using it. The app makes no permission checks of its own. [SECURITY.md](./SECURITY.md) describes what this means.

---

## 4. Data Retention, Deletion & Residency
Because all application state resides in native Jira Project and Issue Entity Properties:
- **Data Residency**: The properties are stored with the rest of the customer's Jira site, in the site's data residency location.
- **Data Deletion**: Deleting a Jira issue or project deletes its `cabQuorumLedger` or `cabQuorumPolicy` property. The issue panel's **Reset Quorum Ledger** button deletes that issue's `cabQuorumLedger`. The app has no retention setting and no uninstall step, so a Jira administrator should delete the properties through Jira's REST API before uninstalling if the data must go.

---

## 5. Vulnerability Disclosure & Support
- **Security reports**: security@evidencepair.com. EvidencePair LLC acknowledges every security report within one business day.
- **Support**: <https://evidencepair.com/support/> (support@evidencepair.com). We reply within one business day.
- **Privacy questions**: hello@evidencepair.com.
