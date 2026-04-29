---
name: nhi-drift-monitor
description: Detect and report changes in an organization's
  non-human identity (NHI) environment since a previous
  assessment baseline. Produces a delta report only — not
  a full inventory assessment. Use when monitoring NHI
  governance posture changes between scan windows, detecting
  new ungoverned credentials, ownership loss, scope
  expansion, rotation overdue, or agentic identity drift.
  Requires a prior baseline from nhi-risk-scorer. Covers
  Entra ID, Okta, CyberArk, Active Directory, AWS IAM,
  HashiCorp Vault, GitHub Actions, and OT/ICS sources.
license: MIT
compatibility: Designed for Claude Code, OpenAI Codex,
  and any Agent Skills compatible client. No system
  packages required. No network access required.
  Operates on provided baseline and current inventory
  inputs only.
metadata:
  author: mike-poulos
  organization: Windval Technology Solutions
  version: "1.2"
  domain: cybersecurity
  subdomain: non-human-identity
  skill-type: delta-monitor
  prerequisite: nhi-risk-scorer
  validated: "true"
  validation-score: 13/13
  compliance: NERC CIP, NIST 800-53, FedRAMP, SOC2
---

# Skill: NHI Drift Monitor

## Version
1.3 — April 2026

## Author
Mike Poulos, Executive Advisor — Cybersecurity
Windval Technology Solutions

## Purpose
Detect and report changes in an organization's
non-human identity (NHI) environment since a
previous assessment baseline. Produces a delta
report — not a full inventory assessment. Only
what changed, why it matters, and what to do
about it.

For full NHI inventory assessment and risk scoring
use nhi-risk-scorer.md. This skill assumes a prior
baseline exists and focuses exclusively on drift
from that baseline.

## Background
NHI governance failures are rarely one-time events.
They accumulate over time through scope creep,
ownership decay, and governance neglect. A service
account that was properly scoped at provisioning
becomes overprivileged when its owner adds
permissions for a project and never removes them.
An OAuth token granted by an active employee becomes
an orphaned credential when that employee leaves.
An API key that was low-privilege at creation becomes
Critical when the application it serves is granted
elevated access.

Point-in-time assessment finds current risk.
Continuous drift detection finds emerging risk
before it becomes a breach.

This skill encodes practitioner judgment for
detecting, classifying, and prioritizing NHI
state changes across IT, cloud, OT/ICS, and
hybrid environments on a recurring basis.

## Parameters

- **BASELINE:** Previous NHI assessment output or
  inventory snapshot to compare against. Accepts
  nhi-risk-scorer.md output, structured CSV/JSON
  inventory, or dated IdP / directory export.
  Required — this skill cannot produce a delta
  without a baseline.
- **CURRENT:** Current NHI inventory from the same
  sources used to produce the baseline. Must cover
  the same source systems for accurate delta
  detection.
- **INPUT SOURCES:** NHI baseline and current
  inventory from one or more of the following:

  Identity Providers: Entra ID, Okta, Ping
  Identity, JumpCloud, OneLogin, ForgeRock,
  Auth0.

  PAM Platforms: CyberArk PAM, BeyondTrust,
  Delinea (Thycotic/Centrify), HashiCorp Vault,
  Azure Key Vault, AWS Secrets Manager,
  GCP Secret Manager.

  Directory Services: Active Directory,
  AWS IAM, GCP IAM, LDAP directories.

  CI/CD Platforms: GitHub Actions, Azure DevOps,
  GitLab CI, Jenkins — pipeline credential drift
  is a high-priority detection target. Coding
  tool contexts expose credentials at 14x the
  rate of general AI usage.

  SIEM Platforms: Splunk, Microsoft Sentinel,
  IBM QRadar, Elastic — required for last used
  timestamp anomaly detection and NHI activity
  monitoring. Environments without SIEM-based
  NHI logging cannot support anomaly detection.

  Cloud IAM Analytics: AWS IAM Access Advisor
  (last used timestamps for IAM roles), Azure AD
  Access Reviews (access certification completion
  status), GCP IAM Recommender (overprivilege
  detection based on actual usage).

  OT/ICS Asset Management: Claroty, Armis,
  Dragos, Nozomi — OT asset management platforms
  that track service account and credential usage
  in OT environments. Standard IdP exports do not
  cover OT/ICS credential usage. These platforms
  are the only reliable source for NHI drift
  detection in Purdue Level 1-2 environments.

- **ENVIRONMENT:** cloud / OT / hybrid /
  enterprise / all
- **COMPLIANCE_CONTEXT:** NERC CIP / FedRAMP /
  SOC2 / NIST 800-53 / none
- **SCAN_INTERVAL:** daily / weekly / monthly —
  used to contextualize urgency of findings.
  A Critical finding on a daily scan is more
  urgent than the same finding on a monthly scan
  because the window of exposure is shorter.

## Process

### Step 1 — Validate Baseline and Current Input
Before comparison:

- Confirm baseline and current inputs cover the
  same source systems — a delta against mismatched
  sources produces false positives and false
  negatives
- Note the baseline date and current date —
  the drift window is the period between them
- If no baseline exists this skill cannot produce
  a delta report — redirect to nhi-risk-scorer.md
  for initial full assessment, then use that
  output as the baseline for subsequent drift
  monitoring
- Flag any source system present in baseline but
  absent in current input — missing sources are
  themselves a finding, not just a data gap.
  A source system that disappears between scans
  may indicate access was revoked, a platform
  was decommissioned, or monitoring was
  intentionally removed. All three warrant
  investigation.

### Step 2 — Detect New NHIs
Identify NHIs present in current input that do
not appear in baseline:

- New service accounts provisioned since baseline
  — Active Directory, Okta, Entra ID, AWS IAM,
  GCP IAM, LDAP
- New API keys created since baseline — HashiCorp
  Vault, AWS Secrets Manager, Azure Key Vault,
  GCP Secret Manager, SaaS platform API key
  registries
- New OAuth tokens granted since baseline —
  Entra ID enterprise app grants, Okta OAuth
  grants, Google Workspace OAuth app access
- New certificates issued since baseline — TLS,
  code signing, client auth, device certificates
- New CI/CD pipeline credentials — GitHub Actions
  secrets, Azure DevOps service connections,
  GitLab CI variables, Jenkins credentials
- New agentic AI identities provisioned since
  baseline — MCP server credentials, LLM API
  keys, Copilot Studio service principals, agent
  service accounts
- New Entra ID app registrations or Google
  Workspace OAuth grants not present in baseline

For each new NHI:
- Classify by type per nhi-risk-scorer.md taxonomy
- Assess privilege scope immediately — new NHIs
  with elevated privilege at provisioning are
  higher risk than those with least privilege
- Flag any new NHI with no owner documentation
  as Critical — undocumented NHIs provisioned
  without an owner are the most common precursor
  to orphaned credential risk
- Cross-reference agentic AI identities across
  all identity sources — a single new agent
  workload may appear as multiple new NHI entries
  across Entra ID, Okta, and PAM platforms
  simultaneously

### Step 3 — Detect State Changes on Existing NHIs
Identify NHIs present in both baseline and current
input where governance state has changed:

**Scope Expansion — High minimum**
Permission scope broader in current than baseline.
Any scope expansion without documented approval
is a governance failure. Detect via:
- Okta application assignment changes
- Entra ID role or permission additions
- AWS IAM policy attachments
- CyberArk safe permission additions
- HashiCorp Vault policy expansions
Elevate to Critical if expansion crosses IT/OT
boundary, adds administrative capability, or
grants access to BES Cyber Assets.

**Owner Change**
Owner field changed since baseline. Not inherently
a risk finding but requires validation — confirm
new owner has accepted accountability. Flag as
Medium if new owner is documented, High if owner
field is now blank or unknown. Cross-reference
against HR systems or Okta user lifecycle data
if available.

**Ownership Loss — Critical**
NHI present in baseline with a documented owner
that now shows no owner. Owner departure without
NHI ownership transfer is the most common source
of orphaned credentials. Cross-reference against
HR termination data, Okta deprovisioning logs,
or Entra ID user lifecycle events if available.

**Rotation Overdue**
NHI present in baseline with a rotation policy
that has not been executed within the required
interval since baseline. Detect via:
- CyberArk rotation compliance reports
- HashiCorp Vault lease expiration logs
- AWS Secrets Manager rotation status
- Azure Key Vault rotation policy compliance
- Okta API token last rotated timestamp
Flag as High if overdue by less than one rotation
cycle, Critical if overdue by more than one
rotation cycle.

**Expiration Approaching**
Certificate or token with an expiration date
within 30 days. Flag as High — expiring
credentials cause outages, not just security
gaps. Sources: Azure Key Vault expiry alerts,
AWS ACM certificate expiry, CyberArk certificate
management, Okta API token expiration.
OT/ICS certificates approaching expiration
are Critical — outage in an OT environment
has operational safety consequences. Claroty
and Armis surface certificate expiry in OT
environments that standard PKI tools miss.

**Monitoring Removed**
NHI present in baseline with monitoring enabled
that now shows no monitoring. Removal of a
monitoring control is a governance regression.
Detect via Splunk saved search removal, Sentinel
analytic rule deletion, or SIEM alert suppression.
Flag as High regardless of privilege scope.

**Last Used Timestamp Anomaly**
NHI showing significantly increased or decreased
usage since baseline without a corresponding
change in documented purpose. Detect via:
- AWS IAM Access Advisor last used data
- Azure AD sign-in logs
- Okta system log API activity
- Splunk or Sentinel NHI activity dashboards
- GCP IAM Recommender usage analysis
Sudden spike may indicate compromise or
unauthorized use. Sudden drop may indicate
abandoned credential still active. Both
warrant investigation.

**Agentic Scope Change**
For agentic AI identities — any change in tool
access, MCP server connections, or permission
scope since baseline. Detect via:
- Copilot Studio admin center permission changes
- Entra ID service principal scope modifications
- Okta OAuth grant scope expansions
- LLM API key permission tier changes
Agentic identities accumulate capability over
time as integrations are added and rarely removed.
Flag all agentic scope expansions as High minimum.

### Step 4 — Detect Removed NHIs
Identify NHIs present in baseline but absent
in current input:

- Confirm removal was intentional — check against
  CyberArk deprovisioning records, Okta
  deactivation logs, Entra ID deletion audit
  logs, or change management records
- Flag unconfirmed removals as Medium — unexpected
  NHI deletion can indicate compromise or
  unauthorized access as readily as clean
  offboarding
- For OT/ICS environments flag any removed NHI
  as requiring explicit confirmation — unexpected
  credential removal in Claroty or Armis-managed
  OT environments may indicate tampering
- Cross-reference against PAM platform
  deprovisioning logs — CyberArk, BeyondTrust,
  and Delinea maintain deprovisioning audit trails
  that confirm intentional removal

### Step 5 — Score Delta Findings
Apply nhi-risk-scorer.md scoring criteria to
each delta finding:

| Score | Label | Criteria |
|---|---|---|
| 1 | Critical | New ungoverned NHI with elevated privilege OR ownership loss on existing NHI OR scope expansion crossing IT/OT boundary OR scope expansion granting access to BES Cyber Assets |
| 2 | High | New NHI with governance gaps OR scope expansion on existing NHI OR rotation overdue OR monitoring removed OR agentic scope change |
| 3 | Medium | New NHI with minor gaps OR owner change requiring validation OR expiration approaching OR unconfirmed NHI removal |
| 4 | Informational | Confirmed clean removal OR minor state change with no governance impact |

Elevation rules from nhi-risk-scorer.md apply —
any finding involving OT/ICS access, regulated
data, BES Cyber Asset access, or agentic identity
scope change elevates to Critical regardless of
other factors.

Elevate any CI/CD pipeline credential finding
to Critical if the repository also contains AI
library imports — coding tool contexts expose
credentials at 14x the rate of general AI usage.

Elevate any NHI to Critical if three or more
governance failures are present simultaneously
— ownership loss, scope expansion, and monitoring
removal occurring together represent systematic
governance breakdown, not isolated gaps. A single
NHI with multiple concurrent failures has a higher
effective blast radius than its individual gaps
suggest because each failure removes a compensating
control that would otherwise limit the impact of
the others.

### Step 6 — Map to Compliance Frameworks
Apply the same framework mapping as
nhi-risk-scorer.md for Critical and High findings.

**NERC CIP — BES Cyber Asset Context:**
NERC CIP control applicability depends on whether
the NHI has access to High, Medium, or Low Impact
BES Cyber Assets as classified in the client's
CIP asset inventory. NHIs with access to High or
Medium Impact BES Cyber Assets trigger the most
stringent CIP obligations:
- CIP-004-7 (Personnel and Training)
- CIP-005-7 (Electronic Security Perimeter)
- CIP-007-6 (Systems Security Management)
- CIP-010-4 (Configuration Change Management)
- CIP-013-2 (Supply Chain Risk Management)

Flag any delta finding that changes the compliance
posture established in the baseline assessment —
a new ungoverned NHI with access to a Medium
Impact BES Cyber Asset is both a governance gap
and a CIP compliance event requiring documented
remediation.

When BES Cyber Asset classification has not been
confirmed for an OT-adjacent NHI finding, treat
the account as Medium Impact for remediation
prioritization purposes until classification is
confirmed. Do not defer remediation pending
classification confirmation — the window between
detection and remediation is the exposure window.
Unclassified OT accounts with Critical governance
gaps should be treated with the same urgency as
confirmed Medium Impact BES Cyber Asset accounts.

### Step 7 — Produce Delta Report
Structured output covering only what changed.
Do not reproduce the full NHI inventory.

## Output Format

This skill produces a delta report, not a full
inventory assessment. Output covers only NHIs
that are new, changed, or removed since the
baseline. Do not list NHIs with no state change.
For full inventory assessment use nhi-risk-scorer.md.

Write in crisp direct declarative sentences. No
marketing language. No filler, minimal narrative.
State findings and actions — nothing else.

### Section 1 — Delta Summary
Five lines maximum:
- Scan window: baseline date to current date
- New NHIs detected: count by risk tier
- State changes detected: count by change type
- Removed NHIs: count, confirmed vs unconfirmed
- Net risk posture change vs baseline: improved /
  degraded / unchanged with one-line explanation

### Section 2 — New NHI Findings
Structured table of new NHIs only, ordered
Critical first:

| NHI ID | Type | Source | Owner | Privilege Scope | Risk | Primary Gap | Action | Urgency |

### Section 3 — State Change Findings
Structured table of existing NHIs with governance
state changes, ordered Critical first:

| NHI ID | Type | Source | Change Type | Baseline State | Current State | Risk | Action | Urgency |

### Section 4 — Removed NHI Findings
List of NHIs removed since baseline with
confirmation status. Flag unconfirmed removals
explicitly. State the source platform where
removal was detected.

### Section 5 — Agentic Identity Delta
Separate section for any agentic AI identity
changes — new agent credentials, scope changes
on existing agent identities, or removed agent
credentials. Apply OWASP Agentic Top 10 mapping
for any High or Critical agentic findings:
ASI02 (Tool Misuse), ASI03 (Identity and
Privilege Abuse), ASI04 (Agentic Supply Chain).

### Section 6 — Compliance Posture Change
One paragraph only if compliance posture has
changed since baseline. State which framework,
which control, and which BES Cyber Asset
classification is implicated by delta findings.
Omit this section entirely if no compliance
posture change detected.

## Quality Benchmark
A high-quality delta report is scannable in
under two minutes and contains only actionable
findings. It does not reproduce the full NHI
inventory. It tells the security team exactly
what changed, why it matters, and what to do.

Poor output:
"Here is the complete current NHI inventory
with all 34 entries assessed against governance
criteria..." — this is a full assessment, not
a delta report. Wrong skill for this output.
Use nhi-risk-scorer.md instead.

Good output:
"Scan window: April 14 to April 21, 2026.
3 new NHIs detected (1 Critical, 2 High).
4 state changes detected (1 ownership loss,
2 scope expansions, 1 rotation overdue).
0 removed NHIs. Net risk posture: degraded.
New Critical: n8n-anthropic-prod-key-02
provisioned April 18 in Entra ID — second
Anthropic API key for same pipeline, no owner
documented in CyberArk or Entra ID, no blast
radius definition, no monitoring in Sentinel.
Likely duplicate provisioning event. Revoke
and consolidate with existing governed key
immediately. ASI03, LLM06, CIP-007-6."

## Known Limitations
- Accuracy depends on baseline and current inputs
  covering identical source systems — mismatched
  sources produce unreliable deltas. Okta as
  baseline source and Entra ID as current source
  for the same environment will produce false
  positives.
- Last used timestamp anomaly detection requires
  SIEM-based NHI activity logging in both baseline
  and current inputs — environments without Splunk,
  Sentinel, or equivalent NHI activity monitoring
  cannot support anomaly detection. AWS IAM Access
  Advisor and GCP IAM Recommender partially
  compensate for cloud IAM roles but do not cover
  all NHI types.
- Agentic identity drift is particularly difficult
  to detect when tool access changes occur at the
  MCP server configuration level rather than at
  the identity credential level — MCP server
  config changes may not surface in Okta, Entra
  ID, or PAM platform exports.
- Unconfirmed NHI removals require CyberArk,
  BeyondTrust, or Delinea deprovisioning records,
  Okta deactivation logs, or formal change
  management records to confirm intent —
  environments without PAM platforms or formal
  change management cannot distinguish clean
  offboarding from unauthorized deletion.
- NERC CIP applicability requires BES Cyber Asset
  classification — the client must provide which
  assets are classified as High, Medium, or Low
  Impact BES Cyber Assets per their CIP asset
  inventory. Without this classification CIP
  control mapping cannot be accurately applied.
  NHIs with access to High or Medium Impact BES
  Cyber Assets have significantly more stringent
  CIP obligations than NHIs accessing Low Impact
  or non-BES assets. Do not assume CIP
  applicability without confirmed asset
  classification.
- OT/ICS NHI drift detection requires Claroty,
  Armis, Dragos, or Nozomi platform data —
  standard IdP and PAM exports do not cover
  credential usage in Purdue Level 1-2
  environments. Environments without OT asset
  management platforms have no visibility into
  NHI drift in OT networks.
- This skill processes client data including
  potentially sensitive identity and access
  information — handle all inputs in accordance
  with client data handling requirements and
  do not retain beyond the engagement session.

## Related Skills
- nhi-risk-scorer.md (full NHI inventory
  assessment — use for initial baseline and
  point-in-time risk scoring)
- ai-bom-inventory.md (AI component inventory
  — surfaces agentic identities that feed into
  NHI drift monitoring)
- soc-decision-audit.md (audit trail for NHI
  drift findings and remediation decisions)

## Version History
| Version | Date | Change |
|---|---|---|
| 1.0 | April 2026 | Initial release |
| 1.1 | April 2026 | Added explicit IdP references (Okta, Ping Identity, JumpCloud, OneLogin, ForgeRock, Auth0), PAM platform references (CyberArk, BeyondTrust, Delinea, HashiCorp Vault, Azure Key Vault, AWS Secrets Manager, GCP Secret Manager), directory services (AWS IAM, GCP IAM, LDAP), CI/CD platforms (GitHub Actions, Azure DevOps, GitLab CI, Jenkins), SIEM platforms (Splunk, Sentinel, QRadar, Elastic), cloud IAM analytics (AWS IAM Access Advisor, Azure AD Access Reviews, GCP IAM Recommender), OT/ICS asset management (Claroty, Armis, Dragos, Nozomi), expanded BES Cyber Asset classification guidance in Known Limitations |
| 1.2 | April 2026 | Added multiple simultaneous governance failure elevation rule to Step 5, added unclassified BES Cyber Asset interim treatment guidance to Step 6 NERC CIP section — findings from validation run against synthetic drift inventory |
| 1.3 | April 2026 | Added YAML frontmatter — Agent Skills open standard compliance (agentskills.io) |
