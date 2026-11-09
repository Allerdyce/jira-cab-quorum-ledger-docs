# Privacy policy: CAB Quorum e-Sign & Rovo Release Ledger for Jira

Updated 9 November 2026

This policy describes the Atlassian Marketplace app **CAB Quorum e-Sign & Rovo Release Ledger for Jira** ("the app"), made by EvidencePair LLC ("EvidencePair", "we"). EvidencePair's company privacy policy, which covers our website and email you send us, is at <https://evidencepair.com/privacy/>.

## In short

- The app runs on Atlassian Forge, inside your Jira site. It declares no external egress and no remote services, and it makes requests only to your site's Jira API. Its readiness audit is a fixed set of rules inside the app. It sends nothing to Rovo or any other AI service.
- It keeps its data in Jira itself, as two Jira entity properties. That data is never sent to EvidencePair.
- It does not store your users' account IDs. Every signature records the same account ID and signer name, which are fixed values built into the app, whoever signs. The personal data it can hold is text: the Meaning of Signature people type, and a copy of the issue's summary.

## What the app stores

**One issue property, `cabQuorumLedger`, per issue where someone has signed a stage or run the readiness audit:**

- the issue key, the project ID and the compliance preset;
- a copy of the issue's summary and priority, taken at the latest signature, or at the first audit if nobody has signed yet;
- a SHA-256 hash of the description's text, and a SHA-256 fingerprint of the issue key, summary, description hash and priority. The description itself is not stored;
- for each signed stage: its number, role name and code, the Meaning of Signature as typed, when it was signed, the fingerprint at that moment, and two SHA-256 hashes that chain it to the stage before;
- with each signature, a signer account ID and a signer name. In this version they are the same for every signature: an account ID and a name from the app's built-in test configuration. The app does not record the account ID of the person who clicks Sign;
- the latest readiness audit, which the app also runs at each signature: the score, the verdict, the label it chose, and the result of each of its six checks;
- whether the signatures are complete and unchanged, the gate status, the final hash of the chain, when the record was last updated, and its size in bytes.

**One project property, `cabQuorumPolicy`, written only from the admin page:**

- the compliance preset and the name of its framework;
- the policy settings: whether signing must follow the stage order, whether drift blocks the workflow, whether a Meaning of Signature is required, a minimum number of signers, whether one person may sign every stage, and the minimum readiness score;
- the four stages, each with its order, ID, role name, code, whether it is required, and its default Meaning of Signature;
- a project key and ID, when the policy was last updated, and its size in bytes.

The admin page is in Jira's global settings, so it has no project of its own. It always reads and writes this property on the project with ID 10035, a fixed value from the app's built-in test configuration. The app does not check Jira's reply, so the page shows a success message even if Jira saved nothing, for example because your site has no project with that ID. The issue panel and the project page read this property on their own project. The workflow validator reads it on the project named in the event Jira sends, or on project 10035 if the event names none. Where the property does not exist, they use the app's built-in default policy: the FDA 21 CFR Part 11 preset, with one person allowed to sign every stage.

**Labels on the issue.** When someone runs the readiness audit, the app removes the labels `rovo-cab-verified` and `cab-drift-quarantined` from the issue, then adds one of them back if the result calls for it. It changes no other issue field, and its screens post no comments.

The app does not look up names, email addresses or profiles. A Meaning of Signature, and the summary the app copies, can contain personal data, and the app stores them as they are.

## What the app reads from Jira

- The issue the panel is open on, or the issue being transitioned: its summary, description, priority, status, issue type, subtasks, issue links and labels. It uses the description only to compute a hash, and uses subtasks and blocking links only for their status.
- That issue's `cabQuorumLedger` property, and the `cabQuorumPolicy` property described above.
- On the project page and the admin page, the same fields and ledger for issue ITH-1, in whichever project you open them. ITH-1 is a fixed issue key from the app's built-in test configuration. If the app cannot read an issue ITH-1 on your site, the pages show built-in sample values instead.

It reads no comments, attachments or user profiles.

## Where the data lives

Entity properties are part of your Jira site's data. Atlassian stores them with the rest of your site, in your site's data residency location, under Atlassian's own security and privacy commitments. The app has no storage of its own: it uses no Forge storage and no Forge SQL, and EvidencePair runs no servers in its path.

## Who can see it

Jira shows an entity property to anyone who can view the issue or project it belongs to, through Jira's REST API. Anyone who can see an issue can therefore read its ledger, including the Meaning of Signature text and the copied summary.

The app sends every request to Jira as the app itself, never as the person using it, so Jira applies the app's permissions to each request. This has two effects:

- Anyone who can open the issue panel can sign a stage, run the audit (which changes the issue's labels) or reset the ledger (which deletes it), without permission to edit the issue.
- The project page shows issue ITH-1's key, summary, status and ledger to anyone who can open the app's page in any project, whether or not they can see ITH-1. The admin page, which Jira shows to Jira administrators, shows the same.

## Retention and deletion

The app keeps both properties until someone deletes them. It has no retention setting, deletes nothing on its own schedule, and has no uninstall step. The Reset Quorum Ledger button in the issue panel deletes that issue's `cabQuorumLedger` property. Deleting an issue or a project deletes its properties. A Jira administrator can delete either property at any time through Jira's REST API, and should do so before uninstalling if the data must go. Labels the audit added stay on issues until someone removes them.

## Account closure

The app does not take part in Atlassian's personal data reporting. It stores no account IDs of your users, so it holds nothing keyed to an account that is closed. Personal data in text, such as a name typed into a Meaning of Signature or included in a copied summary, stays until the property or the issue is deleted.

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
