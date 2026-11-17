# LoopGuard for Jira — Security Policy

_Last updated: November 17, 2026 (app version 3)_

This page describes how SentientApps secures LoopGuard for Jira ("the App"): where it runs, how data is handled and encrypted, how access is controlled, and how we find, fix, and disclose vulnerabilities. For what data the App stores and why, see the [Privacy Policy](PRIVACY.md).

## 1. Architecture

- The App is built **entirely on Atlassian Forge**. Its code runs in the Forge runtime, and its data is stored in Forge SQL, both operated by Atlassian.
- **No external egress.** The App's manifest declares no remote backends and no external fetch, image, or frame permissions, and the Forge platform enforces this. The App makes no outbound requests to SentientApps or any third party.
- **No SentientApps infrastructure.** We run no servers, databases, or cloud accounts for the App, and use no third-party subprocessors.

## 2. Data handling

- The App stores only what it needs: issue IDs and keys, project keys, Atlassian account IDs of the actors behind changes, update counts, estimates, and administrator settings. It stores no names, email addresses, or avatars.
- Field values seen in update events are kept only as **one-way hashes**, used to detect a field flipping back and forth.
- Uploaded Automation rule exports are reduced to rule structure. Web request URLs, headers, bodies, tokens, and email addresses are **stripped before anything is stored**.
- The App's only write to Jira is one issue property (`loopguard.state`) on quarantined issues, plus an optional comment that is off by default. It never edits issue fields.
- **Retention:** activity and alerts are deleted after 30 days; counters, incidents, and usage totals after at most 90 days.
- **Closed accounts:** the App reports stored account IDs weekly through Atlassian's Personal Data Reporting API and erases data for accounts Atlassian reports as closed.

## 3. Encryption

- **In transit:** all traffic is between the App's Forge functions and Atlassian APIs over TLS, inside Atlassian's network.
- **At rest:** App data in Forge SQL is encrypted at rest by Atlassian's Forge platform.
- The App collects **no credentials, API tokens, or secrets** from customers and keeps none in its code or repositories.

## 4. Access control

- The App's admin page is available only to **Jira administrators**, and release actions additionally check the Jira `ADMINISTER` permission as the signed-in user.
- The App requests only the scopes it needs: `read:jira-work`, `write:jira-work`, `storage:app`, and `report:personal-data`.
- SentientApps staff have **no access to customer data**. App logs contain only event names, counts, and error codes (no personal data or tokens), and reach us only if a site administrator allows log sharing.

## 5. Secure development

- All user input is validated with strict schemas (bounded numbers, length limits, pattern-checked identifiers). All database queries use parameterized statements, and Jira search queries are built only from internal numeric IDs, never from user text.
- Every release passes `forge lint`, Atlassian's Forge security scan, type checks, and automated tests.
- Third-party dependencies are scanned with `npm audit` before each release and kept up to date. At the time of writing, it reports 0 known vulnerabilities.

## 6. Vulnerability management

- We review scan results and security reports, and fix vulnerabilities within the timeframes in Atlassian's [Marketplace Security Bug Fix Policy](https://developer.atlassian.com/platform/marketplace/security-bugfix-policy/).
- The App participates in Atlassian's Marketplace security program and responds to findings raised through the Atlassian Marketplace Security (AMS) process.

## 7. Reporting a vulnerability

Please report suspected vulnerabilities privately to **support@sentientapps.io** with the subject "Security", including steps to reproduce. Don't open a public GitHub issue for security problems. We acknowledge reports within 3 business days and keep you informed until the issue is resolved.

## 8. Incident response

If a security incident or critical vulnerability affects customers, we notify Atlassian and affected customers following Atlassian's [Security Incident Management Guidelines](https://developer.atlassian.com/platform/marketplace/app-security-incident-management-guidelines/), and publish details and remediation steps here once it is safe to do so.

## 9. Certifications

SentientApps holds no third-party security certifications (such as SOC 2 or ISO 27001) at this time. Because the App runs entirely on Atlassian Forge, hosting, storage, and network security are provided by Atlassian's platform, which holds its own certifications. See the [Atlassian Trust Center](https://www.atlassian.com/trust).
