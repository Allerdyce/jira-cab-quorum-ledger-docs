# Privacy policy: CAB Quorum e-Sign & Rovo Release Ledger for Jira

Updated 10 November 2026

This policy describes the Atlassian Marketplace app **CAB Quorum e-Sign & Rovo Release Ledger for Jira** ("the app"), made by EvidencePair LLC ("EvidencePair", "we"). EvidencePair's company privacy policy, which covers our website and email you send us, is at <https://evidencepair.com/privacy/>.

## In short

- The app runs on Atlassian Forge, inside your Jira site. It declares no external egress and no remote services, and it makes requests only to your site's Jira API. Its readiness audit is a fixed set of rules inside the app. It sends nothing to Rovo or any other AI service.
- It keeps its data in Jira itself, as two Jira entity properties. That data is never sent to EvidencePair.
- It stores the Atlassian account ID of each person who signs a stage. It stores no names, email addresses or profiles. The other personal data it can hold is text: the Meaning of Signature people type, and a copy of the issue's summary.

## What the app stores

**One issue property, `cabQuorumLedger`, per issue where someone has signed a stage or run the readiness audit:**

- the issue key, the project ID and the compliance preset;
- a copy of the issue's summary and priority, taken at the latest signature, or at the first audit if nobody has signed yet;
- a SHA-256 hash of the description's text, and a SHA-256 fingerprint of the issue key, summary, description hash and priority. The description itself is not stored;
- for each signed stage: its number, role name and code, the Meaning of Signature as typed, when it was signed, the fingerprint at that moment, and two SHA-256 hashes that chain it to the stage before;
- with each signature, the Atlassian account ID of the person who signed it. The app takes it from Atlassian's platform when the person clicks Sign. It stores no name with it. Signatures made by earlier versions of the app instead hold a fixed account ID and name from the app's built-in test configuration, whoever signed; the Reset Quorum Ledger button removes them;
- the latest readiness audit, which the app also runs at each signature: the score, the verdict, the label it chose, and the result of each of its six checks;
- whether the signatures are complete and unchanged, the gate status, the final hash of the chain, when the record was last updated, and its size in bytes.

**One project property, `cabQuorumPolicy`, which the app reads but never writes:**

- the compliance preset and the name of its framework;
- the policy settings: whether signing must follow the stage order, whether drift blocks the workflow, whether a Meaning of Signature is required, a minimum number of signers, whether one person may sign every stage, and the minimum readiness score;
- the four stages, each with its order, ID, role name, code, whether it is required, and its default Meaning of Signature;
- a project key and ID, when the policy was last updated, and its size in bytes.

Writing a project property needs a Jira permission the app does not ask for, so this version has no way to change a project's policy. A project has this property only if someone saved it through Jira's REST API. The issue panel and the workflow validator read it on the issue's own project, and the project page reads it on its own project. The admin page is in Jira's global settings and has no project, so it reads no project's policy. Where the property does not exist, the app uses its built-in default policy: the FDA 21 CFR Part 11 preset, with one person allowed to sign every stage.

**Labels on the issue.** When someone runs the readiness audit, the app removes the labels `rovo-cab-verified` and `cab-drift-quarantined` from the issue, then adds one of them back if the result calls for it. It changes no other issue field, and its screens post no comments. The change is made as the person who ran the audit, so Jira's history shows them as its author.

The app does not look up names, email addresses or profiles. To show who signed, the issue panel uses Jira's own user display, which shows the person's current name from the stored account ID; the app does not receive or store that name. A Meaning of Signature, and the summary the app copies, can contain personal data, and the app stores them as they are.

## What the app reads from Jira

- The issue the panel is open on, or the issue being transitioned: its summary, description, priority, status, issue type, subtasks, issue links, labels and project. It uses the description only to compute a hash, uses subtasks and blocking links only for their status, and uses the project only to find its policy.
- That issue's `cabQuorumLedger` property, and the `cabQuorumPolicy` property described above.
- Whether the person using the issue panel may edit the issue, which Jira answers for that person.
- On the project page, the summary, description, priority, status, subtasks, issue links and `cabQuorumLedger` property of the project's most recently updated issues, up to 500, to list the ones that have a ledger. It uses them the same way as above.

The admin page reads no issues. The app reads no comments, attachments or user profiles. It has no built-in sample issue: if Jira does not return what it asked for, the screen shows Jira's error.

## Where the data lives

Entity properties are part of your Jira site's data. Atlassian stores them with the rest of your site, in your site's data residency location, under Atlassian's own security and privacy commitments. The app has no storage of its own: it uses no Forge storage and no Forge SQL, and EvidencePair runs no servers in its path.

## Who can see it

Jira shows an entity property to anyone who can view the issue or project it belongs to, through Jira's REST API. Anyone who can see an issue can therefore read its ledger, including the signers' account IDs, the Meaning of Signature text and the copied summary.

The app's screens send every request to Jira as the person using them, so Jira applies that person's own permissions:

- Signing a stage, running the audit (which changes the issue's labels) and resetting the ledger (which deletes it) need permission to edit the issue. The app checks this with Jira first and refuses if the person does not have it.
- The project page lists only issues the person can see in Jira.

The workflow validator runs as the app, because Jira runs it during a transition with no signed-in person. It reads the issue, its ledger and the policy, and writes nothing.

## Retention and deletion

The app keeps both properties until someone deletes them. It has no retention setting, deletes nothing on its own schedule, and has no uninstall step. The Reset Quorum Ledger button in the issue panel deletes that issue's `cabQuorumLedger` property, including the account IDs of everyone who signed it. Deleting an issue or a project deletes its properties. A Jira administrator can delete either property at any time through Jira's REST API, and should do so before uninstalling if the data must go. Labels the audit added stay on issues until someone removes them.

## Account closure

The app stores the account IDs of people who sign. It does not yet take part in Atlassian's personal data reporting, so it does not learn when an account is closed and does not remove a closed account's ID from the ledgers that person signed. The ID stays until someone resets the ledger or deletes the issue. Personal data in text, such as a name typed into a Meaning of Signature or included in a copied summary, also stays until the property or the issue is deleted.

## Platform logs

The app writes nothing to the Forge platform's logs. Atlassian's platform can still record errors it sees while the app runs, such as an unhandled error and its message ([Atlassian: view app logs](https://developer.atlassian.com/platform/forge/view-app-logs/)).

## Email and support

If you email us, our [company privacy policy](https://evidencepair.com/privacy/) describes how we handle that email, and the current restrictions on where our apps may be used.

## What we never do

We do not sell personal data or use it for unrelated advertising or AI training. The app's data stays in your Jira site and never reaches us.

## Changes to this policy

The date at the top changes when this policy does. Every version is kept in this repository's history.

## Contact

hello@evidencepair.com
