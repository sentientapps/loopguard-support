# LoopGuard for Jira — Privacy Policy

_Last updated: October 31, 2026 (app version 3)_

LoopGuard for Jira ("the App"), provided by SentientApps, detects runaway Jira Automation loops and quarantines the looping issue. The App runs **natively on Atlassian Forge** and introduces **no external servers, no external databases, and no third-party subprocessors**.

## 1. Where processing happens

All processing runs **inside the Atlassian Forge runtime**. The App's manifest declares **no external egress**, and the Forge platform enforces this, so your data **never leaves Atlassian** during processing.

The App reads:

- **Issue update events** from Jira: the issue ID, its project key, the time, the Atlassian account ID of the person or app that made the change, and which fields changed with their old and new values. Field values are used only to detect a field flipping back and forth; they are stored as one-way hashes, never as text.
- **Recently updated issues and their change history**, every 5 minutes, to attribute changes to issues, projects, and actors.
- **An Automation rules export**, only if an administrator uploads one. The App keeps rule structure only (name, trigger, components, projects, schedule, and JQL with email addresses removed). It strips web request URLs, headers, bodies, tokens, and email addresses before storing anything.

## 2. What is stored, and where

All App data is stored **within Atlassian**, in Forge SQL. **No external database is used.**

- **Activity and alerts.** Issue keys, project keys, the Atlassian account ID of each actor, change counts, and estimated Automation steps and Rovo credits. Kept 30 days.
- **Real-time counters.** Per-issue, per-minute update counts, with actors and field changes stored only as one-way hashes. Counters that did not trip are deleted after 48 hours; counters that tripped are deleted after 90 days.
- **Incidents.** The issue ID, when it was quarantined, the update count, and when it was released. Deleted 90 days after release.
- **Settings.** Guardrails per project, allowlisted Atlassian account IDs and project keys, alert recipients (an account ID or group name), usage allowances, and preferences.
- **Imported rule structure**, as described above, until an administrator deletes it.
- **Usage totals and forecast alerts.** Daily estimated totals. Kept 90 days.
- **Issue property.** On a quarantined issue, the App sets the issue property `loopguard.state` with `quarantined`, the time, the reason, and the update count. It is removed when the issue is released or the quarantine expires.
- **Optional comment.** Only if an administrator turns it on, the App adds one comment to a quarantined issue explaining the quarantine.

The App stores **no user profile data** (names, email addresses, or avatars). It never edits an issue's status, summary, labels, or other fields.

**Logs.** The App's Forge logs contain issue IDs, counts, and decisions (for example, that an issue was quarantined). They stay in Atlassian and reach SentientApps only if a site administrator allows log sharing when installing the App.

**Analytics.** The App sends no analytics and records no custom metrics.

## 3. Personal data and closed accounts

The only personal data the App stores is **Atlassian account IDs**. Every week the App reports the account IDs it holds through Atlassian's Personal Data Reporting API. When Atlassian reports an account as closed, the App deletes that account's activity, alerts, dry runs, allowlist entries, alert routes, and cached actor type, and clears it from imported rule structure.

## 4. Data in transit and encryption

All traffic is between the App's Forge functions and Atlassian's own APIs, over TLS, inside Atlassian's network. Data at rest is protected by Atlassian's encryption of Forge storage. **No data is transmitted to SentientApps or any third party.**

## 5. Subprocessors

The App uses **no third-party subprocessors**: no external AI services, no external APIs, and no cloud accounts operated by SentientApps. It relies solely on the Atlassian Forge platform.

## 6. Uninstalling

When the App is uninstalled, Atlassian deletes the App's Forge SQL data according to Forge data retention. **Before uninstalling, click Release all** on the Circuit Breaker tab: Jira can remove the App's access before its uninstall cleanup runs, so a `loopguard.state` property can otherwise remain on an issue until an administrator deletes it.

## 7. Contact

Questions about privacy or security: open an issue in this repository. Please don't include personal data or secrets in the issue.
