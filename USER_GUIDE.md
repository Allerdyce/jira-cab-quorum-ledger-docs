# User & Administrator Guide — CAB Quorum e-Sign & Rovo Release Ledger for Jira

## 1. Overview
**CAB Quorum e-Sign & Rovo Release Ledger for Jira** provides regulated enterprise software, medical device, financial services, and critical infrastructure teams with a cryptographically verifiable Change Advisory Board (CAB) e-Signature ledger directly inside Jira and Jira Service Management.

---

## 2. Configuring Global & Project Governance Policies (`Enterprise CAB Quorum & Non-Repudiation Policy`)
1. Navigate to **Apps -> Enterprise CAB Quorum & Non-Repudiation Policy** (`jira:adminPage`).
2. Select your active regulatory governance preset:
   - **FDA 21 CFR Part 11 Electronic Signatures** (`§11.50 / §11.70 / §11.200`)
   - **SOX Section 404 ITGC Change Management** (`PCAOB AS 2201`)
   - **HIPAA Security Rule Production Release** (`45 CFR §164.308 / §164.312`)
   - **EU DORA & ISO/IEC 27001:2022 A.8.32 Change Control**
3. Review the ordered 4-stage CAB role matrix stored in Jira Project Property `cabQuorumPolicy` (`<32 KB`).

---

## 3. Signing the 4-Stage Merkle CAB Matrix on Jira Issues (`CAB Quorum e-Sign & Rovo Release Auditor`)
1. Open any Jira issue or JSM Change / Service Request (e.g., `ITH-1`) and open the **CAB Quorum e-Sign & Rovo Release Auditor** panel.
2. Verify the **Live Payload Fingerprint** (`SHA256(issueKey|summary|descriptionDigest|priority)`).
3. Enter or confirm the mandatory **Meaning of Signature** attestation and click **Sign Stage N in Sequence**:
   - **Stage 1**: Engineering Lead — Code & Rollback Verified
   - **Stage 2**: QA & Automated Test Lead — Regression Passed
   - **Stage 3**: Security & Compliance Officer — SAST/SCA & Zero PII Leak
   - **Stage 4**: CAB Chair — Production Release Authorized
4. Click **Run Rovo AI Pre-Flight Audit** to compute the 6-point Release Readiness Score (`0–100%`) and apply the `rovo-cab-verified` label.

---

## 4. Payload-Drift Tamper Detection & Remediation
If a user modifies the ticket's `summary`, `description`, or `priority` after any CAB stage has been signed:
- The live Payload Fingerprint no longer matches the signed fingerprint in `cabQuorumLedger`.
- The Issue Panel and Workflow Validator immediately transition to `BLOCKED_PAYLOAD_DRIFT_DETECTED` and apply label `cab-drift-quarantined`.
- Click **Reset Quorum Ledger** to clear the invalidated chain and re-attest Stages 1–4 against the updated ticket scope.

---

## 5. Exporting 21 CFR Part 11 Certificates & RFC-4180 CSV Ledgers (`CAB Release Ledger & 21 CFR Part 11 Certificates`)
1. Navigate to **Project Sidebar -> CAB Release Ledger & 21 CFR Part 11 Certificates** (`jira:projectPage`).
2. Inspect the project-wide Merkle Release Ledger table.
3. Click **View 21 CFR Part 11 Certificate** to inspect the full cryptographic chain (`Genesis -> Stage 1 -> Stage 2 -> Stage 3 -> Stage 4`) or click **Export RFC-4180 CSV Ledger** for external audit submission.
