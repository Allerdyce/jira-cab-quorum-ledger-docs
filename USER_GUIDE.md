# User & Administrator Guide — CAB Quorum e-Sign & Rovo Release Ledger for Jira

## 1. Overview
**CAB Quorum e-Sign & Rovo Release Ledger for Jira** provides regulated enterprise software, medical device, financial services, and critical infrastructure teams with a cryptographically verifiable Change Advisory Board (CAB) e-Signature ledger directly inside Jira and Jira Service Management.

---

## 2. Governance Policies (Jira settings, Apps, and each project's page)
1. Each project has its own CAB policy, stored in the Jira project property `cabQuorumPolicy` (`<32 KB`). A project with no saved policy uses the default: the **FDA 21 CFR Part 11 Electronic Signatures** preset (`§11.50 / §11.70 / §11.200`), where one person may sign every stage.
2. To see a project's policy, open **Project sidebar -> CAB Quorum e-Sign & Rovo Release Ledger for Jira** (`jira:projectPage`). It shows the preset, whether one person may sign every stage, and the ordered 4-stage CAB role matrix.
3. **Apps -> CAB Quorum e-Sign & Rovo Release Ledger for Jira** in Jira settings (`jira:adminPage`) has no project. It shows the default policy and the presets the app knows:
   - **FDA 21 CFR Part 11 Electronic Signatures** (`§11.50 / §11.70 / §11.200`)
   - **SOX Section 404 ITGC Change Management** (`PCAOB AS 2201`)
   - **HIPAA Security Rule Production Release** (`45 CFR §164.308 / §164.312`)
   - **EU DORA & ISO/IEC 27001:2022 A.8.32 Change Control**
4. Changing a project's policy is not available in this version. Writing a project property needs a Jira permission the app does not ask for.

---

## 3. Signing the 4-Stage Merkle CAB Matrix on Jira Issues (issue panel)
1. Open any Jira issue or JSM Change / Service Request and open the **CAB Quorum e-Sign & Rovo Release Ledger for Jira** panel.
2. Verify the **Live Payload Fingerprint** (`SHA256(issueKey|summary|descriptionDigest|priority)`).
3. Enter or confirm the mandatory **Meaning of Signature** attestation and click **Sign Stage N in Sequence**. The signature records your Atlassian account, and the **Signed By** column shows your name:
   - **Stage 1**: Engineering Lead — Code & Rollback Verified
   - **Stage 2**: QA & Automated Test Lead — Regression Passed
   - **Stage 3**: Security & Compliance Officer — SAST/SCA & Zero PII Leak
   - **Stage 4**: CAB Chair — Production Release Authorized
4. If the project's policy lets one person sign every stage, **1-Click Seal All 4 CAB Stages** signs every remaining stage as you, with each stage's default Meaning of Signature. If the policy needs a different person for each stage, the button is hidden and the app refuses a second signature from the same account.
5. Click **Run Pre-Flight Audit** to compute the 6-point Release Readiness Score (`0–100%`) and apply the `rovo-cab-verified` label. The audit is a fixed set of rule checks; it makes no AI or Rovo call. If Jira refuses the label change (for example, the Labels field is not on the issue's edit screen), the audit is still saved and the panel says the label was not changed.
6. Signing, running the audit and resetting the ledger need permission to edit the issue. Without it, the panel is read-only and says so.

---

## 4. Payload-Drift Tamper Detection & Remediation
If a user modifies the ticket's `summary`, `description`, or `priority` after any CAB stage has been signed:
- The live Payload Fingerprint no longer matches the signed fingerprint in `cabQuorumLedger`.
- The Issue Panel and Workflow Validator immediately transition to `BLOCKED_PAYLOAD_DRIFT_DETECTED`. Running the pre-flight audit then applies the label `cab-drift-quarantined`.
- Click **Reset Quorum Ledger** to clear the invalidated chain and re-attest Stages 1–4 against the updated ticket scope.

---

## 5. Exporting 21 CFR Part 11 Certificates & RFC-4180 CSV Ledgers (project page)
1. Navigate to **Project Sidebar -> CAB Quorum e-Sign & Rovo Release Ledger for Jira** (`jira:projectPage`).
2. Inspect the project-wide Merkle Release Ledger table. It lists the project's issues that have a CAB ledger, among the 500 most recently updated issues you can see.
3. Click **View 21 CFR Part 11 Certificate** to inspect the cryptographic chain (`Genesis -> Stage 1 -> Stage 2 -> Stage 3 -> Stage 4`) of the most recently updated issue in the table, or click **Export RFC-4180 CSV Ledger** for external audit submission. The CSV lists each stage's signer by Atlassian account ID.
