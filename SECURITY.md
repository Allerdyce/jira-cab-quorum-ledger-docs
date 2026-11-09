# Security: CAB Quorum e-Sign & Rovo Release Ledger for Jira

Updated 9 November 2026

How the Atlassian Marketplace app **CAB Quorum e-Sign & Rovo Release Ledger for Jira**, made by EvidencePair LLC, handles data, what protects it, how we handle vulnerabilities, and which certifications apply. What the app stores is listed in full in its [privacy policy](./PRIVACY_POLICY.md).

## Data handling

- **Forge only.** The app runs on Atlassian Forge (`nodejs24.x`) on Atlassian infrastructure.
- **No egress.** The manifest declares no external permissions and no remote services. The app makes requests only to your site's Jira API, and it loads no analytics, error-reporting services or external assets. The readiness audit is a fixed set of rules inside the app and calls no Rovo or other AI service.
- **No storage of its own.** The app keeps its data in two Jira entity properties on your site (`cabQuorumPolicy` on a project, `cabQuorumLedger` on an issue). It uses no Forge storage or Forge SQL, and EvidencePair has no copy.
- **Every request runs as the app.** All of the app's calls to Jira, including those from its screens, run as the app. Jira applies the app's permissions to them, and the app makes no permission checks of its own. So anyone who can open the issue panel can sign a stage, run the audit or reset the ledger. The project page and the admin page show issue ITH-1, a fixed key from the app's built-in test configuration, to anyone who can open them. The workflow validator reads the issue, its ledger and the policy property, and writes nothing.
- **Permissions it asks for.**
  - `read:jira-work`: read the issue's fields, its `cabQuorumLedger` property and the `cabQuorumPolicy` property.
  - `write:jira-work`: write and delete `cabQuorumLedger`, set the two audit labels on the issue, and write `cabQuorumPolicy` from the admin page. The app does not check whether Jira accepted that last write.
  - The manifest declares no other scopes.
- **What the hashes are.** Each signature carries a SHA-256 hash of the previous stage's hash, the issue's fingerprint, the signer account ID, the role, the Meaning of Signature and the time. The chain shows whether the stored record still matches itself, and whether the issue's summary, description or priority changed after a stage was signed. It is not a digital signature: no secret key is involved, and someone with permission to edit the issue property could change the record and recompute every hash.
- **Who signed.** In this version every signature records the same fixed account ID and name from the app's built-in test configuration, so the record does not show who signed. The default policy lets one account sign all four stages. The panel's 1-Click Seal All 4 CAB Stages button signs every remaining stage at once, with each stage's default Meaning of Signature.

## Encryption

The app stores nothing outside your Jira site and sends nothing outside Atlassian's platform. Encryption in transit and at rest is provided by Atlassian as part of your site, as described in the [Atlassian Trust Center](https://www.atlassian.com/trust). The app adds no encryption of its own.

## Vulnerability management

- **Reporting.** Email security@evidencepair.com. Tell us the app, what you found and how to reproduce it. Please do not test against sites you do not own. We acknowledge every report within one business day.
- **Response.** EvidencePair adopted a written security incident response plan on 12 October 2026. It covers triage, containment, investigation, notification of Atlassian and affected customers, and recovery.
- **Dependencies.** The app's npm dependencies are pinned in its lockfile. Atlassian maintains and patches the Forge runtime.
- **Bug bounty.** We do not run a bug bounty programme.

## Certifications

EvidencePair holds no security certifications, and the app has had no independent penetration test or external audit. The infrastructure the app runs on is Atlassian's, and Atlassian's own certifications cover it: see the [Atlassian Trust Center](https://www.atlassian.com/trust).
