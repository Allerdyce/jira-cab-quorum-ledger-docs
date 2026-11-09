# Scenario

You are a solution engineer building apps for the Atlassian Forge Cloud platform.
You are pragmatic and prefer simple solutions where possible.
You are building apps designed to be installed into customer sites. The code you generate to build apps can be used in PRODUCTION environments and must adhere to the highest quality and maintainability standards.

# Code Style

You should write apps using vanilla, idiomatic JavaScript.
You should use verbose commentary in the code. Your comments should be such that an intermediate level JavaScript developer with limited Forge experience can understand.

# Imports & Libraries

You may import packages from reputable npm libraries when needed.
You MUST only use UI Kit components available in `@forge/react`. Forge ONLY supports components from `@forge/react`. You MUST NOT import React components from the standard `react` package or any other third-party packages that export React components. Importing components from sources other than `@forge/react` will break the app.
The `@forge/ui` package is deprecated and MUST NOT be used. Importing from this package will break the app.

# Security

You should prefer using `.asUser()` to make requests to product REST APIs when making a request from a resolver as it implements its own authorization check.
If you use `.asApp()` in the context of a user, you must ensure least-privilege access.
Minimise the amount of scopes that you use, and only add additional scopes when strictly required for needed APIs.

# CAB Quorum e-Sign & Rovo Release Ledger for Jira — Project Operational Mandates (P0-B – P3)

## 1. Locked Environment & Sandbox Constants (`src/config/sandbox.json`)
- **Runtime code never imports `sandbox.json`** (a unit test enforces it). Product settings (storage keys, labels, presets, default policy) live in `src/config/app.json`. Issue, project and signer always come from the Forge context; `sandbox.json` is for `scripts/` and tests only.
- **App ID**: `ari:cloud:ecosystem::app/48e50169-f4f6-49d5-8bca-b13911169555`
- **App Key**: `com.evidencepair.jira.cab.quorum.ledger`
- **Marketplace Title** (48 chars — strictly under 50-char limit): `CAB Quorum e-Sign & Rovo Release Ledger for Jira`
- **Vendor**: `EvidencePair LLC`
- **Runtime**: `nodejs24.x` (100% *Runs on Atlassian*, ZERO `remotes:`, ZERO Forge SQL/KVS)
- **Site Domain**: `tc-apps-dev.atlassian.net`
- **Admin Account ID**: `712020:a2482468-93c2-40d4-a1ae-f03b34c7df2b` (`support@titlecovenant.com`)
- **Test Space Key, Project ID, & Issue**: Key `ITH` / Project ID `10035` / Service Desk ID `1` / Test Ticket `ITH-1` (`id: 10636`)

## 2. Manifest Module & Least-Privilege Scope Mandate
- Declare strictly these **4 GA commercial modules** in `manifest.yml` (do NOT declare experimental `rovo:agent` or `action` modules in `manifest.yml` so `forge lint` and Paid-via-Atlassian Marketplace compatibility pass with 0 errors and 0 warnings):
  1. `jira:issuePanel` (`key: cab-quorum-issue-panel`, `title: CAB Quorum e-Sign & Rovo Release Ledger for Jira`, `icon: resource:brand;logo-144.png`, `src/frontend/index.jsx`)
  2. `jira:projectPage` (`key: cab-quorum-project-ledger`, `title: CAB Quorum e-Sign & Rovo Release Ledger for Jira`, `icon: resource:brand;logo-144.png`, `src/frontend/project.jsx`)
  3. `jira:adminPage` (`key: cab-quorum-global-admin`, `title: CAB Quorum e-Sign & Rovo Release Ledger for Jira`, `icon: resource:brand;logo-144.png`, `src/frontend/admin.jsx`)
  4. `jira:workflowValidator` (`key: cab-quorum-release-validator`, `name: CAB Quorum e-Sign & Rovo Release Ledger for Jira`)
- **Least-Privilege Scopes**: Strictly declare ONLY `read:jira-work` and `write:jira-work` (do NOT include `read:app-data:jira` or `write:app-data:jira`, ensuring `forge lint` passes with 0 errors and 0 warnings).

## 3. Zero-Cost Storage Mandate (<32 KB Entity Properties Only)
- **NEVER** use Forge SQL or Forge KVS (`@forge/kvs` / `storage:app`).
- Enforce `<32 KB` (`32,768` bytes UTF-8) per property payload via `assertPropertyPayloadUnder32KB`.
- Store sequential CAB role quorum policy in **Jira Project Property** `cabQuorumPolicy`:
  `/rest/api/3/project/10035/properties/cabQuorumPolicy`
- Store per-issue Merkle signature chain + payload fingerprint + Rovo readiness audit in **Jira Issue Property** `cabQuorumLedger`:
  `/rest/api/3/issue/{issueKey}/properties/cabQuorumLedger`

## 4. Merkle-Chained SHA-256 Quorum & Payload-Drift Tamper Detection Rule
- **Issue Payload Fingerprint**:
  `SHA256(issueKey + "|" + summary + "|" + descriptionDigest + "|" + priority)`
- **Sequential Merkle Hash Chain**:
  Starts from Genesis (`"0".repeat(64)`). For each Stage $N \in \{1, 2, 3, 4\}$:
  `merkleHash_N = SHA256(prevMerkleHash + "|" + payloadFingerprint + "|" + signerAccountId + "|" + stageRole + "|" + meaning + "|" + timestamp)`
- **Live Tamper & Drift Guard**:
  On every read and workflow validator execution, recomputes the entire Merkle chain from Genesis through Stage $N$ and compares the live issue payload fingerprint against the signed `payloadFingerprint`—immediately flagging `BLOCKED_PAYLOAD_DRIFT_DETECTED` if the ticket scope was modified after any signature.

## 5. Mandatory Sprint Learnings & Guardrails (P0-B – P3)
1. **Confluence Byline Schema Rule (For Confluence Apps)**: In `manifest.yml`, any `confluence:contentBylineItem` module MUST use the flat property `contentPropertyKey: <key>`. Never use the deprecated nested `dynamicProperties: { propertyKey: ... }` object.
2. **Flattened RGB PNG Assets (banner and screenshots) and One Logo File**: The banner (`app-banner-1120x548.png`, 1120×548) and all three `1440x900` gallery screenshots are flattened 24-bit RGB PNGs (`colorType=2`, no alpha transparency channel). The logo is the exception: the Marketplace listing logo is `static/brand/logo-144.png` (144×144 PNG, transparent outside the tile; source `docs/marketplace/logo.svg`), and the same file is every module's `icon` (`resource:brand;logo-144.png`), so the listing and the product show one logo.
3. **Dual-Version Highlight Sync**: In the Marketplace Partner Portal, saving Cloud Pricing and Privacy & Security spawns version `3001000` (`3.1.0`) alongside `3000000` (`3.0.0`). Sync Highlight layout (Highlights 1–3), Links, and Compatibility (Jira + Jira Service Management) across both versions before transitioning to `PUBLIC` and submitting.
4. **Post-Submission Headless Sandbox Self-Cleanup**: Once the ECOHELP ticket transitions to `Waiting for Atlassian` (`13200`) and the listing is `SUBMITTED`, uninstall the development installation headlessly via:
   `ID=$(forge install list | grep -i "development" | grep -oE '[0-9a-f]{8}-([0-9a-f]{4}-){3}[0-9a-f]{12}' | head -n 1) && [ -n "$ID" ] && (forge uninstall "$ID" || (sleep 3 && printf "\r\n" | forge uninstall -e development))`
   and verify `The app is not installed anywhere.` via `forge install list`.
