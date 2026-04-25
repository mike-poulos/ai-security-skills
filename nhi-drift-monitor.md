# Skill: NHI Drift Monitor

## Version
1.0 — April 2026

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
  inventory, or dated Entra ID / Google Workspace
  export. Required — this skill cannot produce a
  delta without a baseline.
- **CURRENT:** Current NHI inventory from the same
  sources used to produce the baseline. Must cover
  the same source systems for accurate delta
  detection.
- **ENVIRONMENT:** cloud / OT / hybrid / enterprise / all
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
- If no baseline exists, this skill cannot produce
  a delta report — redirect to nhi-risk-scorer.md
  for initial full assessment, then use this output
  as the baseline for subsequent drift monitoring
- Flag any source systems present in baseline but
  absent in current input — missing sources are
  themselves a finding, not just a data gap

### Step 2 — Detect New NHIs
Identify NHIs present in current input that do
not appear in baseline:

- New service accounts provisioned since baseline
- New API keys created since baseline
- New OAuth tokens granted since baseline
- New certificates issued since baseline
- New agentic AI identities provisioned since
  baseline — MCP server credentials, LLM API
  keys, agent service principals
- New Entra ID app registrations or Google
  Workspace OAuth grants since baseline

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

### Step 3 — Detect State Changes on Existing NHIs
Identify NHIs present in both baseline and current
input where governance state has changed:

**Scope Expansion — High minimum**
Permission scope broader in current than baseline.
Any scope expansion without documented approval
is a governance failure. Elevate to Critical if
expansion crosses IT/OT boundary or adds
administrative capability.

**Owner Change**
Owner field changed since baseline. Not inherently
a risk finding but requires validation — confirm
new owner has accepted accountability. Flag as
Medium if new owner is documented, High if owner
field is now blank or unknown.

**Ownership Loss — Critical**
NHI present in baseline with a documented owner
that now shows no owner. Owner departure without
NHI ownership transfer is the most common source
of orphaned credentials. Cross-reference against
HR termination data if available.

**Rotation Overdue**
NHI present in baseline with a rotation policy
that has not been executed within the required
interval since baseline. Flag as High if overdue
by less than one rotation cycle, Critical if
overdue by more than one rotation cycle.

**Expiration Approaching**
Certificate or token with an expiration date
within 30 days. Flag as High — expiring
credentials cause outages, not just security
gaps. OT/ICS certificates approaching expiration
are Critical — outage in an OT environment has
operational safety consequences.

**Monitoring Removed**
NHI present in baseline with monitoring enabled
that now shows no monitoring. Removal of a
monitoring control is a governance regression.
Flag as High regardless of privilege scope.

**Last Used Timestamp Anomaly**
NHI showing significantly increased or decreased
usage since baseline without a corresponding
change in documented purpose. Sudden spike may
indicate compromise or unauthorized use. Sudden
drop may indicate abandoned credential still
active. Both warrant investigation.

**Agentic Scope Change**
For agentic AI identities — any change in tool
access, MCP server connections, or permission
scope since baseline. Agentic identities
accumulate capability over time as integrations
are added and rarely removed. Flag all agentic
scope expansions as High minimum.

### Step 4 — Detect Removed NHIs
Identify NHIs present in baseline but absent
in current input:

- Confirm removal was intentional — check against
  deprovisioning records or change management logs
  if available
- Flag unconfirmed removals as Medium — unexpected
  NHI deletion can indicate compromise or
  unauthorized access as readily as clean
  offboarding
- For OT/ICS environments flag any removed NHI
  as requiring explicit confirmation — unexpected
  credential removal in OT environments may
  indicate tampering

### Step 5 — Score Delta Findings
Apply nhi-risk-scorer.md scoring criteria to
each delta finding:

| Score | Label | Criteria |
|---|---|---|
| 1 | Critical | New ungoverned NHI with elevated privilege OR ownership loss on existing NHI OR scope expansion crossing IT/OT boundary |
| 2 | High | New NHI with governance gaps OR scope expansion on existing NHI OR rotation overdue OR monitoring removed |
| 3 | Medium | New NHI with minor gaps OR owner change requiring validation OR expiration approaching |
| 4 | Informational | Confirmed clean removal OR minor state change with no governance impact |

Elevation rules from nhi-risk-scorer.md apply —
any finding involving OT/ICS access, regulated
data, or agentic identity scope change elevates
to Critical regardless of other factors.

### Step 6 — Map to Compliance Frameworks
Apply the same framework mapping as
nhi-risk-scorer.md for Critical and High findings.
Flag any delta finding that changes the
compliance posture established in the baseline
assessment — a new ungoverned NHI with CIP-005-7
implications is a compliance state change, not
just a governance gap.

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
Four lines maximum:
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

| NHI ID | Type | Change Type | Baseline State | Current State | Risk | Action | Urgency |

### Section 4 — Removed NHI Findings
List of NHIs removed since baseline with
confirmation status. Flag unconfirmed removals
explicitly.

### Section 5 — Agentic Identity Delta
Separate section for any agentic AI identity
changes — new agent credentials, scope changes
on existing agent identities, or removed agent
credentials. Apply OWASP Agentic Top 10 mapping
for any High or Critical agentic findings.

### Section 6 — Compliance Posture Change
One paragraph only if compliance posture has
changed since baseline. State which framework
and which control is newly implicated by delta
findings. Omit this section entirely if no
compliance posture change detected.

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

Good output:
"Scan window: April 14 to April 21, 2026.
3 new NHIs detected (1 Critical, 2 High).
4 state changes detected (1 ownership loss,
2 scope expansions, 1 rotation overdue).
0 removed NHIs. Net risk posture: degraded.
New Critical: n8n-anthropic-prod-key-02
provisioned April 18 — second Anthropic API
key for same pipeline, no owner documented,
no blast radius definition, no monitoring.
Likely duplicate provisioning event. Revoke
and consolidate with existing governed key
immediately. ASI03, LLM06."

## Known Limitations
- Accuracy depends on baseline and current inputs
  covering identical source systems — mismatched
  sources produce unreliable deltas
- Last used timestamp anomaly detection requires
  usage logging to be present in both baseline
  and current inputs — environments without NHI
  activity logging cannot support this detection
- Agentic identity drift is particularly difficult
  to detect when tool access changes occur at the
  MCP server configuration level rather than at
  the identity credential level — MCP server
  config changes may not surface in standard
  identity exports
- Unconfirmed NHI removals require change
  management or deprovisioning records to
  confirm intent — environments without formal
  change management cannot distinguish clean
  offboarding from unauthorized deletion
- NERC CIP applicability requires BES Cyber Asset
  classification — assumes classification is
  provided or inferable from input
- This skill processes client data — handle all
  inputs in accordance with client data handling
  requirements and do not retain beyond the
  engagement session

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
