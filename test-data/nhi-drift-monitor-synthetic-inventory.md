# Synthetic NHI Drift Monitor Test Inventory
# nhi-drift-monitor.md Validation

## Purpose
Synthetic two-state NHI inventory for validating
nhi-drift-monitor.md skill file output quality.
Contains a baseline snapshot and a current state
snapshot with deliberate drift planted across all
detection categories. Ground truth validation
checklist at bottom confirms expected delta
report output.

## Organization
Fictional Energy Co. (FEC) — Hybrid IT/OT Environment

## Sources Simulated
- Microsoft Entra ID (primary IdP)
- Okta (secondary IdP — acquired subsidiary)
- CyberArk PAM (privileged credential management)
- Active Directory (on-premises service accounts)
- AWS IAM (cloud workload identities)
- HashiCorp Vault (secrets management)
- GitHub Actions (CI/CD pipeline credentials)
- Rockwell FactoryTalk / OSIsoft PI (OT/ICS)
- Agentic AI Identities (Anthropic API, Copilot Studio)

## Scan Window
Baseline: April 14, 2026
Current: April 21, 2026
Interval: Weekly

## Suggested Parameters for Test Run
- ENVIRONMENT: hybrid
- COMPLIANCE_CONTEXT: NERC CIP
- SCAN_INTERVAL: weekly

---

## SECTION 1 — BASELINE SNAPSHOT
### State as of April 14, 2026

### Entra ID — Service Principals and App Registrations

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| SP-001 | Service Principal | FEC-ThreatIntel-Pipeline | Entra ID | Security Team | Anthropic API — read/write | 30 days ago | Yes | Governed. n8n threat intel pipeline. Low risk. |
| SP-002 | Service Principal | PowerBI-DataGateway-Prod | Entra ID | IT Admin | Files.ReadWrite.All + Sites.ReadWrite.All | 90 days ago | Yes | Documented. Monitored. Acceptable use case. |
| SP-003 | Service Principal | Copilot-OT-AdvisoryAgent | Entra ID | Security Team | Sites.Read.All + SecurityEvents.Read + Teams.ReadWrite | 60 days ago | Yes | Agentic. Blast radius defined. Human override documented. |
| SP-004 | Service Principal | GitHub-Actions-CICD | Entra ID | DevOps Team | Application.ReadWrite + RoleManagement.ReadWrite.Directory | 45 days ago | Yes | Governed. CI/CD pipeline. Scope reviewed. |

### Okta — Service Accounts (Acquired Subsidiary)

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| OK-001 | Service Account | svc-okta-scim-sync | Okta | IT Ops | SCIM provisioning — User.ReadWrite | 60 days ago | Yes | Governed. User provisioning sync. |
| OK-002 | Service Account | svc-okta-hr-integration | Okta | HR Systems | HR attribute sync — Profile.ReadWrite | 45 days ago | Yes | Governed. HR system integration. |
| OK-003 | OAuth Token | salesforce-okta-sso | Okta | IT Admin | SSO + User.Read | 180 days ago | Yes | SSO integration. Long rotation cycle noted but documented. |

### CyberArk PAM — Privileged Accounts

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| CA-001 | Service Account | svc-backup-veeam | CyberArk | IT Ops | Local admin on backup targets | 30 days ago | Yes | Governed. Veeam backup service account. Rotation enforced by CyberArk. |
| CA-002 | Service Account | svc-sccm-push | CyberArk | Desktop Engineering | Local admin on all Windows endpoints | 30 days ago | Yes | Governed. SCCM software deployment. |
| CA-003 | Service Account | svc-scada-historian | CyberArk | OT Team | OSIsoft PI historian read/write — OT Level 2-3 | 30 days ago | Yes | Governed. OT environment. CIP-007-6 compliant. Rotation enforced. |

### Active Directory — Service Accounts

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| AD-001 | Service Account | svc-sql-reporting | Active Directory | DBA Team | SQL Server read — reporting databases | 90 days ago | Yes | Governed. Read-only SQL reporting. |
| AD-002 | Service Account | svc-jumpserver-ot | Active Directory | OT Team | RDP access to OT DMZ jump server — IT/OT boundary | 45 days ago | Yes | Governed. CIP-005-7 ESP access. Monitored in Sentinel. |
| AD-003 | Service Account | svc-factorytalk-svc | Active Directory | OT Team | Rockwell FactoryTalk service account — PLC Level 1-2 | 60 days ago | Yes | Governed. OT Level 1-2. CIP-007-6 compliant. |

### AWS IAM — Cloud Service Accounts

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| AWS-001 | IAM Role | fec-lambda-dataprocessing | AWS IAM | Cloud Team | S3 read/write — data processing bucket only | N/A (role) | Yes | Governed. Least privilege. IAM role — no rotation required. |
| AWS-002 | IAM User | svc-terraform-deploy | AWS IAM | DevOps Team | PowerUser — infrastructure deployment | 45 days ago | Yes | Governed. Terraform deployment. Scoped to dev/staging. |

### HashiCorp Vault — Secrets

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| HV-001 | API Key | anthropic-api-key-prod | HashiCorp Vault | Security Team | LLM inference — threat intel pipeline | 30 days ago | Yes | Governed. Auto-rotation configured. |
| HV-002 | API Key | snowflake-etl-key | HashiCorp Vault | Data Engineering | Snowflake data warehouse — ETL read/write | 45 days ago | Yes | Governed. Lease-based rotation. |

### GitHub Actions — CI/CD Credentials

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| GH-001 | Secret | AZURE_DEPLOY_TOKEN | GitHub Actions | DevOps Team | Azure deployment — scoped to prod resource group | 60 days ago | Yes | Governed. Scoped deployment credential. |
| GH-002 | Secret | SONAR_TOKEN | GitHub Actions | DevOps Team | SonarQube code analysis — read only | 90 days ago | Yes | Governed. Code quality scanning. |

### OT/ICS — Vendor and Platform Accounts

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| OT-001 | Vendor Remote Access | rockwell-vendor-remote | Vendor Account | OT Team | Rockwell remote diagnostic — PLC Level 1 | Never | No | Known gap. Vendor-managed. FEC has no visibility into rotation. Documented in CIP-013-2 vendor risk register. |
| OT-002 | Service Account | svc-osisoft-pi-connector | OSIsoft PI | OT Team | PI data historian — OT Level 2 read | 90 days ago | Yes | Governed. PI connector service account. |
| OT-003 | Certificate | cert-ot-vpn-gateway | OT DMZ | OT Team | OT remote access VPN — OT DMZ | Issued 18 months ago | Yes | Expiry: October 2026. Within acceptable window at baseline. |

---

## SECTION 2 — CURRENT STATE SNAPSHOT
### State as of April 21, 2026
### Changes from baseline appear in Notes column

### Entra ID — Service Principals and App Registrations

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| SP-001 | Service Principal | FEC-ThreatIntel-Pipeline | Entra ID | Security Team | Anthropic API — read/write | 37 days ago | Yes | No change. |
| SP-002 | Service Principal | PowerBI-DataGateway-Prod | Entra ID | Unknown | Files.ReadWrite.All + Sites.ReadWrite.All + Reports.ReadWrite.All | 97 days ago | Yes | DRIFT: Owner field now blank — original owner departed. Scope expanded — Reports.ReadWrite.All added since baseline. |
| SP-003 | Service Principal | Copilot-OT-AdvisoryAgent | Entra ID | Security Team | Sites.Read.All + SecurityEvents.Read + Teams.ReadWrite + Mail.Read | 67 days ago | Yes | DRIFT: Mail.Read scope added since baseline. Agentic scope expansion — now reads all mailboxes. No documented approval. |
| SP-004 | Service Principal | GitHub-Actions-CICD | Entra ID | DevOps Team | Application.ReadWrite + RoleManagement.ReadWrite.Directory | 52 days ago | Yes | No change. |
| SP-005 | Service Principal | Zapier-FinanceIntegration | Entra ID | Unknown | Gmail.modify + Drive.ReadWrite + Contacts.ReadWrite | Never | No | NEW: Not in baseline. No owner. Processing Finance data. Gmail.modify scope. Never rotated. |

### Okta — Service Accounts (Acquired Subsidiary)

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| OK-001 | Service Account | svc-okta-scim-sync | Okta | IT Ops | SCIM provisioning — User.ReadWrite | 67 days ago | Yes | No change. |
| OK-002 | Service Account | svc-okta-hr-integration | Okta | Unknown | HR attribute sync — Profile.ReadWrite + Salary.Read | 52 days ago | No | DRIFT: Owner field now blank. Salary.Read scope added. Monitoring removed. |
| OK-003 | OAuth Token | salesforce-okta-sso | Okta | IT Admin | SSO + User.Read | 187 days ago | Yes | No change. |
| OK-004 | Service Account | svc-okta-audit-export | Okta | Unknown | Okta system log export — AuditLog.Read | Never | No | NEW: Not in baseline. No owner. Never rotated. No monitoring. Audit log export capability — sensitive. |

### CyberArk PAM — Privileged Accounts

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| CA-001 | Service Account | svc-backup-veeam | CyberArk | IT Ops | Local admin on backup targets | 37 days ago | Yes | No change. |
| CA-002 | Service Account | svc-sccm-push | CyberArk | Desktop Engineering | Local admin on all Windows endpoints including OT HMIs | 37 days ago | Yes | DRIFT: Scope expanded — now includes OT HMI endpoints. Crosses IT/OT boundary. CIP-005-7 implication. |
| CA-003 | Service Account | svc-scada-historian | CyberArk | OT Team | OSIsoft PI historian read/write — OT Level 2-3 | 30 days ago | Yes | No change. |

### Active Directory — Service Accounts

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| AD-001 | Service Account | svc-sql-reporting | Active Directory | DBA Team | SQL Server read — reporting databases | 97 days ago | Yes | No change. |
| AD-002 | Service Account | svc-jumpserver-ot | Active Directory | OT Team | RDP access to OT DMZ jump server — IT/OT boundary | 52 days ago | Yes | No change. |
| AD-003 | Service Account | svc-factorytalk-svc | Active Directory | Unknown | Rockwell FactoryTalk service account — PLC Level 1-2 | 67 days ago | No | DRIFT: Owner field now blank — OT team lead departed. Monitoring removed from Sentinel. OT Level 1-2 access. CIP-007-6 implication. |
| AD-004 | Service Account | svc-dcs-emerson | Active Directory | Unknown | Emerson DeltaV DCS — Level 1 process control | Never | No | NEW: Not in baseline. No owner. Never rotated. No monitoring. DCS Level 1 access — direct process control capability. |

### AWS IAM — Cloud Service Accounts

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| AWS-001 | IAM Role | fec-lambda-dataprocessing | AWS IAM | Cloud Team | S3 read/write — data processing bucket only | N/A (role) | Yes | No change. |
| AWS-002 | IAM User | svc-terraform-deploy | AWS IAM | DevOps Team | AdministratorAccess — all AWS resources | 52 days ago | Yes | DRIFT: Scope expanded from PowerUser to AdministratorAccess. Full AWS blast radius. No documented approval in change management. |

### HashiCorp Vault — Secrets

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| HV-001 | API Key | anthropic-api-key-prod | HashiCorp Vault | Security Team | LLM inference — threat intel pipeline | 37 days ago | Yes | No change. |
| HV-002 | API Key | snowflake-etl-key | HashiCorp Vault | Data Engineering | Snowflake data warehouse — ETL read/write | 52 days ago | Yes | No change. |
| HV-003 | API Key | openai-api-key-finance | HashiCorp Vault | Unknown | OpenAI GPT-4o — Finance data processing | Never | No | NEW: Not in baseline. No owner. Never rotated. No monitoring. Finance data submitted to external AI model. |

### GitHub Actions — CI/CD Credentials

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| GH-001 | Secret | AZURE_DEPLOY_TOKEN | GitHub Actions | DevOps Team | Azure deployment — scoped to prod resource group | 67 days ago | Yes | No change. |
| GH-002 | Secret | SONAR_TOKEN | GitHub Actions | DevOps Team | SonarQube code analysis — read only | 97 days ago | Yes | No change. |
| GH-003 | Secret | ANTHROPIC_API_KEY | GitHub Actions | Unknown | Anthropic API — fec-scada-analytics repository | Never | No | NEW: Not in baseline. Found in fec-scada-analytics CI/CD pipeline. No owner. SCADA telemetry data submitted to Anthropic API via pipeline. Credential in AI-integrated repository — 14x exposure risk. CIP-010-4, CIP-013-2. |

### OT/ICS — Vendor and Platform Accounts

| NHI_ID | Type | Name | Source | Owner | Permission_Scope | Last_Rotated | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|
| OT-001 | Vendor Remote Access | rockwell-vendor-remote | Vendor Account | OT Team | Rockwell remote diagnostic — PLC Level 1 | Never | No | No change. Known gap. |
| OT-002 | Service Account | svc-osisoft-pi-connector | OSIsoft PI | OT Team | PI data historian — OT Level 2 read | 97 days ago | Yes | No change. |
| OT-003 | Certificate | cert-ot-vpn-gateway | OT DMZ | OT Team | OT remote access VPN — OT DMZ | Issued 18 months ago | Yes | DRIFT: Expiry now 28 days away — crossed 30-day threshold since baseline. High finding. OT remote access outage risk. |
| OT-004 | Vendor Remote Access | emerson-vendor-remote | Vendor Account | Unknown | Emerson DeltaV remote diagnostic — DCS Level 1 | Never | No | NEW: Not in baseline. No owner. Never rotated. No monitoring. DCS Level 1 access. Vendor-managed — FEC has no rotation visibility. CIP-013-2. |

---

## Ground Truth Validation Checklist

### New NHIs — Expected in Section 2 of Delta Report

| NHI_ID | Source | Expected Score | Key Finding |
|---|---|---|---|
| SP-005 | Entra ID | Critical | No owner, Finance data, Gmail.modify scope, never rotated, no monitoring |
| AD-004 | Active Directory | Critical | No owner, DCS Level 1 process control access, never rotated, no monitoring — OT elevation |
| GH-003 | GitHub Actions | Critical | SCADA telemetry to Anthropic API, credential in AI-integrated repo — 14x exposure risk, CIP-010-4 |
| HV-003 | HashiCorp Vault | Critical | No owner, Finance data to OpenAI, never rotated, no monitoring |
| OK-004 | Okta | High | No owner, audit log export capability, never rotated, no monitoring |
| OT-004 | Vendor Account | High | No owner, DCS Level 1 vendor remote access, never rotated, CIP-013-2 |

### State Changes — Expected in Section 3 of Delta Report

| NHI_ID | Change Type | Expected Score | Key Finding |
|---|---|---|---|
| SP-002 | Ownership loss + scope expansion | Critical | Owner departed, Reports.ReadWrite.All added — dual governance failure |
| AD-003 | Ownership loss + monitoring removed | Critical | OT team lead departed, OT Level 1-2 FactoryTalk access, monitoring removed from Sentinel — CIP-007-6 |
| CA-002 | Scope expansion — IT/OT boundary | Critical | SCCM scope expanded to OT HMI endpoints — crosses IT/OT boundary, CIP-005-7 |
| SP-003 | Agentic scope expansion | High | Mail.Read added to OT advisory agent — reads all mailboxes, no documented approval, ASI03 |
| OK-002 | Ownership loss + scope expansion + monitoring removed | Critical | Owner departed, Salary.Read added, monitoring removed — triple governance failure |
| AWS-002 | Scope expansion — full admin | Critical | PowerUser to AdministratorAccess — full AWS blast radius, no change management record |
| OT-003 | Expiration approaching | High | OT VPN certificate expiry now 28 days — crossed 30-day threshold, OT remote access outage risk |

### Removed NHIs — Expected in Section 4 of Delta Report

None planted in this inventory. Absence of removals is itself a valid test — delta report Section 4 should state zero removals detected with no unconfirmed removals.

### Self-Inventory
Delta report must confirm the AI agent executing
the assessment is not itself an NHI that drifted
between baseline and current state.

---

## Expected Delta Summary

- Scan window: April 14 to April 21, 2026
- New NHIs: 6 (4 Critical, 2 High)
- State changes: 7 (5 Critical, 2 High)
- Removed NHIs: 0
- Net risk posture: Degraded — 9 Critical findings
  vs 0 Critical in baseline

---

## Version History

| Version | Date | Notes |
|---|---|---|
| 1.0 | April 2026 | Initial release — weekly drift simulation across Microsoft-centric hybrid IT/OT environment with Okta, CyberArk, AWS IAM, HashiCorp Vault, GitHub Actions, and OT/ICS sources |
