# Skill: NHI Risk Scorer

## Version
1.2 — April 2026

## Author
Mike Poulos, Executive Advisor — Cybersecurity  
Windval Technology Solutions

## Purpose
Score and prioritize non-human identity (NHI) risk across 
an organization's service accounts, API keys, OAuth tokens, 
certificates, and agentic AI identities. Produces a 
prioritized remediation queue with governance gap analysis 
mapped to applicable compliance frameworks.

## Background
Non-human identities outnumber human identities in most 
enterprise environments by 100:1 and are governed at a 
fraction of the rigor applied to human accounts. Service 
accounts with excessive privilege, API keys with no 
rotation policy, OAuth tokens with persistent broad scope, 
and agentic AI identities with ungoverned tool access 
represent the fastest-growing and least-visible attack 
surface in enterprise security.

This skill encodes practitioner judgment for assessing, 
scoring, and prioritizing NHI risk across IT, cloud, 
OT/ICS, and hybrid environments.

## Parameters

- **INPUT:** NHI inventory — service account list, API key 
  registry, OAuth token list, certificate inventory, or 
  agent identity manifest. Accepts structured (CSV/JSON) 
  or unstructured (text description) input.
- **ENVIRONMENT:** cloud / OT / hybrid / enterprise / all
- **COMPLIANCE_CONTEXT:** NERC CIP / FedRAMP / SOC2 / 
  NIST 800-53 / none
- **SCOPE:** service-accounts / api-keys / oauth-tokens / 
  certificates / ai-agents / all

## Process

### Step 1 — Classify Each NHI by Type
Identify and classify each identity in the input by type:
- Service account (Windows/AD, Linux, cloud IAM)
- API key (internal service, third-party integration, 
  vendor-managed)
- OAuth token (user-delegated, machine-to-machine, 
  persistent grant)
- Certificate (TLS, code signing, client auth, device)
- Agentic AI identity (MCP server credential, LLM 
  API key, agent service principal, skill execution 
  context)
- Before scoring, cross-reference agentic AI identities 
  across all identity sources in the inventory — a single 
  agent workload may appear as multiple NHI entries 
  (service principal + API key + OAuth grant). Treat 
  correlated entries as a single blast radius assessment.

### Step 2 — Assess Privilege Scope
For each NHI assess what it can access:
- Data sensitivity: what data stores, systems, or 
  environments does this identity have access to
- Action scope: read-only vs. read-write vs. 
  administrative vs. destructive capability
- Blast radius: if this identity is compromised, 
  what is the maximum impact — lateral movement 
  paths, data exfiltration potential, operational 
  disruption risk
- Cross-boundary exposure: does this identity 
  cross IT/OT boundaries, environment boundaries 
  (dev/prod), or organizational boundaries 
  (third-party access)

### Step 3 — Assess Governance Posture
For each NHI evaluate governance controls:
- **Rotation:** documented rotation policy with 
  enforcement evidence — never rotated is Critical gap
- **Monitoring:** usage logged, anomaly alerted, 
  and reviewed — no monitoring is High gap
- **Documentation:** owner, purpose, expiration, and 
  system dependency documented — undocumented is 
  High gap
- **Scoping:** identity scoped to least privilege — 
  overprivileged is High gap
- **Expiration:** expiration date enforced — 
  no expiration is Medium gap
- **Binding:** identity bound to specific source IP, 
  workload, or context — unbound is Medium gap

### Step 4 — Score Risk
Assign a risk score to each NHI using this matrix:

| Score | Label | Criteria |
|---|---|---|
| 1 | Critical | High privilege + no rotation + no monitoring + cross-boundary exposure |
| 2 | High | High privilege + one or more governance gaps |
| 3 | Medium | Limited privilege + governance gaps present |
| 4 | Low | Least privilege + documented + monitored + rotated |

Elevate any NHI to Critical regardless of privilege 
if it has direct access to OT/ICS environments, 
production financial systems, or regulated data stores.

Treat any agentic AI identity as High minimum. 
Tool access chains and MCP server permissions are 
routinely under-scoped at provisioning — actual 
blast radius is determined at runtime, not at 
credential issuance.

### Step 5 — Map to Compliance Frameworks
Map each Critical and High finding to applicable 
controls across all relevant frameworks:

**NERC CIP:**
- CIP-004-7 (Personnel and Training — access management)
- CIP-005-7 (Electronic Security Perimeter — NHI 
  access paths into ESP)
- CIP-007-6 (Systems Security Management — account 
  management, default accounts)
- CIP-010-4 (Configuration Change Management — 
  undocumented service accounts as configuration gap)
- CIP-013-2 (Supply Chain — third-party NHI access)

**NIST 800-53 / FedRAMP:**
- AC-2 (Account Management)
- AC-6 (Least Privilege)
- IA-2 (Identification and Authentication)
- IA-5 (Authenticator Management — rotation)
- AU-2 (Audit Events — NHI activity logging)

**NIST CSF 2.0:**
- GV.SC (Supply Chain Risk — third-party NHI access)
- PR.AA (Identity Management and Authentication)
- PR.DS (Data Security — credential protection)
- DE.CM (Continuous Monitoring — NHI activity)

**MITRE ATT&CK:**
- T1078 (Valid Accounts — compromised service accounts)
- T1552 (Unsecured Credentials — plaintext storage)
- T1098 (Account Manipulation — privilege escalation)
- T1528 (Steal Application Access Token — OAuth abuse)

**MITRE ATLAS:**
- AML.T0047 (ML-Enabled Product Abuse — agentic 
  identity exploitation)
- AML.T0040 (ML Inference API Access — LLM API 
  key compromise)

**OWASP LLM Top 10 2025:**
- LLM06 (Excessive Agency — overprivileged agent 
  identities)
- LLM02 (Sensitive Information Disclosure — 
  credential exposure via LLM output)

**OWASP Agentic Top 10 2026:**
- ASI02 (Tool Misuse and Exploitation — ungoverned 
  tool access via agent identity)
- ASI03 (Identity and Privilege Abuse — agent 
  identity overprivilege)
- ASI04 (Agentic Supply Chain Vulnerabilities — 
  third-party agent and MCP server identities)

**SOC2:**
- CC6.1 (Logical access security)
- CC6.2 (Prior to issuing credentials)
- CC6.3 (Role-based access)

### Step 6 — Produce Prioritized Remediation Queue
Output a structured table ordered by risk score with:
- NHI identifier (anonymized if needed)
- NHI type
- Risk score and label
- Primary governance gap driving the score
- Compliance framework mapping
- Specific recommended remediation action
- Remediation urgency: Immediate / 72 hours / 
  30 days / scheduled

## Output Format

Write in crisp direct declarative sentences. No marketing 
language. No filler, minimal narrative. State findings 
and actions — nothing else.

### Section 1 — Executive Summary
Two to three sentences maximum. State the total NHI 
count assessed, count by risk tier, and the single 
highest-priority finding with its blast radius 
implication.

### Section 2 — Remediation Queue Table
Structured table ordered Critical first:

| NHI ID | Type | Risk | Primary Gap | Framework | Action | Urgency |

### Section 3 — Governance Gap Summary
One paragraph per gap category (rotation, monitoring, 
documentation, scoping, expiration, binding). Identify 
systemic gaps vs. isolated issues. No narrative — 
findings and patterns only.

### Section 4 — Agentic Identity Findings
Separate section for any AI agent identities identified. 
Assess tool access scope, data access, and whether the 
agent identity has defined blast radius limits, 
escalation logic, and human override mechanisms.

Map to OWASP Agentic Top 10: ASI02 (Tool Misuse), 
ASI03 (Identity and Privilege Abuse), ASI04 
(Agentic Supply Chain).

## Quality Benchmark
A high-quality output names specific NHI identifiers, 
states exact privilege scope, identifies the specific 
lateral movement or data exposure path enabled by the 
governance gap, and produces remediation actions 
specific enough to assign to an engineer.

Poor output:
"Service account has excessive privileges — 
reduce scope."

Good output:
"svc-iconics-historian (SQL Server service account) 
stores credentials in plaintext in 
C:\ProgramData\ICONICS\Cache\*.sdf — any local 
low-privilege user can harvest and authenticate to 
SQL backend. Switch to Windows Authentication 
immediately. Rotate credential. Restrict local login 
to admin-only. CIP-007-6 R5, AC-2, IA-5, T1552, 
ASI03."

## Known Limitations
- Accuracy depends on completeness of input inventory — 
  ungoverned NHIs by definition may not appear in 
  any inventory
- Agentic AI identity assessment requires tool 
  definitions and MCP server configurations not 
  always present in standard inventories
- NERC CIP applicability requires BES Cyber Asset 
  classification — assumes classification is provided 
  or inferable from input
- Agentic deployments frequently generate multiple NHI 
  entries across identity systems that are not correlated 
  in standard inventories — for example, a Copilot Studio 
  agent may appear as both an Entra ID service principal 
  and a separate application credential, each assessed 
  independently but representing the same blast radius. 
  Before scoring, correlate agentic identities across 
  all identity sources to avoid underestimating combined 
  scope and privilege.

## Related Skills
- ai-bom-inventory.md (agentic identity enumeration)
- soc-decision-audit.md (audit trail for NHI risk 
  decisions)
- nerc-cip-gap-analysis.md (broader CIP compliance 
  gap analysis)

## Version History
| Version | Date | Change |
|---|---|---|
| 1.0 | April 2026 | Initial release |
| 1.1 | April 2026 | Updated NHI ratio to 100:1, tightened agentic identity elevation language, added MITRE ATT&CK, MITRE ATLAS, NIST CSF 2.0, OWASP LLM Top 10, OWASP Agentic Top 10 framework mappings, added output voice instruction |
| 1.2 | April 2026 | Added agentic identity correlation guidance to Step 1 and Known Limitations — finding from validation run against synthetic inventory |
