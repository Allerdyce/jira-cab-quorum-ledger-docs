# Security: CAB Quorum e-Sign & Rovo Release Ledger for Jira

Updated 10 November 2026

How the Atlassian Marketplace app **CAB Quorum e-Sign & Rovo Release Ledger for Jira**, made by EvidencePair LLC, handles data, what protects it, how we handle vulnerabilities, and which certifications apply. What the app stores is listed in full in its [privacy policy](./PRIVACY_POLICY.md).

## Data handling

- **Forge only.** The app runs on Atlassian Forge (`nodejs24.x`) on Atlassian infrastructure.
- **No egress.** The manifest declares no external permissions and no remote services. The app makes requests only to your site's Jira API, and it loads no analytics, error-reporting services or external assets. The readiness audit is a fixed set of rules inside the app and calls no Rovo or other AI service.
- **No storage of its own.** The app keeps its data in two Jira entity properties on your site (`cabQuorumPolicy` on a project, `cabQuorumLedger` on an issue). It uses no Forge storage or Forge SQL, and EvidencePair has no copy.
- **The screens act as the person using them.** Every request the issue panel, the project page and the admin page send to Jira runs as the signed-in person, so Jira applies that person's own permissions.
- **Editing the issue is required to change anything.** Before it signs a stage, resets the ledger or runs the audit, the app asks Jira whether the person may edit the issue. If they may not, the app refuses and writes nothing. People who can only view the issue see the ledger read-only.
- **The workflow validator acts as the app.** It runs inside a Jira transition, where there is no signed-in person. It reads the issue, its ledger and its project's policy property, and writes nothing.
- **Failures are shown.** The app checks every reply from Jira. If a read or a write fails, the screen shows Jira's error. The app has no built-in sample issue or project to fall back on.
- **Permissions it asks for.**
  - `read:jira-work`: read the issue's fields, its `cabQuorumLedger` property, the `cabQuorumPolicy` property, the project's issues for the project page, and the person's own permission on the issue.
  - `write:jira-work`: write and delete `cabQuorumLedger`, and set the two audit labels on the issue.
  - The manifest declares no other scopes. The app does not write `cabQuorumPolicy`: changing a project's policy needs a Jira scope the app does not ask for, so this version has no way to change it.
- **What the hashes are.** Each signature carries a SHA-256 hash of the previous stage's hash, the issue's fingerprint, the signer account ID, the role, the Meaning of Signature and the time. The chain shows whether the stored record still matches itself, and whether the issue's summary, description or priority changed after a stage was signed. It is not a digital signature: no secret key is involved, and someone with permission to edit the issue property could change the record and recompute every hash.
- **Who signed.** Each signature records the Atlassian account ID of the person who signed. The app takes it from Atlassian's platform, not from anything the browser sends. The app stores no name; the panel shows each signer's current Jira name from the account ID.
- **One person can sign every stage by default.** The default policy lets one account sign all four stages. Only then does the panel show the 1-Click Seal All 4 CAB Stages button, which signs every remaining stage as the person who clicks it, with each stage's default Meaning of Signature. If a project has a saved `cabQuorumPolicy` that requires a different person for each stage, the app hides that button and refuses a second signature from the same account. The app never writes that property; a project has one only if someone saved it through Jira's REST API.

## Encryption

The app stores nothing outside your Jira site and sends nothing outside Atlassian's platform. Encryption in transit and at rest is provided by Atlassian as part of your site, as described in the [Atlassian Trust Center](https://www.atlassian.com/trust). The app adds no encryption of its own.

## Vulnerability management

- **Reporting.** Email security@evidencepair.com. Tell us the app, what you found and how to reproduce it. Please do not test against sites you do not own. We acknowledge every report within one business day.
- **Response.** EvidencePair adopted a written security incident response plan on 12 October 2026. It covers triage, containment, investigation, notification of Atlassian and affected customers, and recovery.
- **Dependencies.** The app's npm dependencies are pinned in its lockfile. Atlassian maintains and patches the Forge runtime.
- **Bug bounty.** We do not run a bug bounty programme.

## Certifications

EvidencePair holds no security certifications, and the app has had no independent penetration test or external audit. The infrastructure the app runs on is Atlassian's, and Atlassian's own certifications cover it: see the [Atlassian Trust Center](https://www.atlassian.com/trust).
