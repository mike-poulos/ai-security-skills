# Synthetic NHI Test Inventory — nhi-risk-scorer.md Validation

## Purpose
Synthetic NHI inventory for validating nhi-risk-scorer.md 
skill file output quality. Contains deliberate governance 
gaps across all NHI types with known expected scores for 
ground truth validation.

## Organization
Fictional Energy Co. (FEC) — Hybrid IT/OT Environment

## Sources Simulated
- Microsoft Entra ID (Service Principals, OAuth Tokens)
- Google Workspace (Service Accounts, OAuth App Grants)
- SaaS Application API Keys (Salesforce, ServiceNow, 
  Slack, GitHub)
- Agentic AI Identities (Anthropic API, Copilot Studio)

## How to Use
1. Copy the inventory table below
2. Open Claude.ai in a new conversation
3. Paste nhi-risk-scorer.md skill file content
4. Set parameters and paste inventory as input
5. Compare output against Ground Truth Validation 
   Checklist at bottom of this file

## Suggested Parameters for Test Run
- ENVIRONMENT: hybrid
- COMPLIANCE_CONTEXT: NERC CIP
- SCOPE: all

---

## NHI Inventory

### Microsoft Entra ID — Service Principals

| NHI_ID | Source | Type | Name | Application/Purpose | Granted_By | Grant_Date | Last_Used | Expiration | Permission_Scope | Owner_Documented | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| SP-001 | Entra ID | Service Principal | Salesforce-Entra-SSO | Salesforce SSO integration | IT Admin | 847 days ago | Yesterday | None | User.Read.All + Group.Read.All + Directory.Read.All | Yes | No | Directory.Read.All exposes full org chart and group membership — significantly broader than SSO requires. Never reviewed since provisioning. |
| SP-002 | Entra ID | Service Principal | PowerBI-DataGateway-Prod | Power BI enterprise data gateway | IT Admin | 1,100 days ago | 3 days ago | None | Files.ReadWrite.All + Sites.ReadWrite.All + Reports.Read.All | No | No | Files.ReadWrite.All across all SharePoint — original owner left organization. No documented purpose review since provisioning. |
| SP-003 | Entra ID | Service Principal | GitHub-Actions-CICD | GitHub Actions pipeline for infrastructure deployments | DevOps Team | 180 days ago | Today | None | Application.ReadWrite.All + RoleManagement.ReadWrite.Directory | Yes | No | RoleManagement.ReadWrite.Directory allows granting any Entra ID role to any principal — effectively Global Admin equivalent capability. |
| SP-004 | Entra ID | Service Principal | Zoom-CalendarIntegration | Zoom calendar and contact sync | Unknown | 730 days ago | 45 days ago | None | Calendars.ReadWrite + Contacts.ReadWrite + Mail.Read | No | No | Mail.Read included — no documented business justification. Owner unknown. Last used 45 days ago — potentially abandoned. |
| SP-005 | Entra ID | Service Principal | Backup-AzureVault-Prod | Azure Backup vault service principal | IT Admin | 365 days ago | Today | None | Contributor on all production Azure subscriptions | Yes | No | Subscription-level Contributor — can modify or delete any Azure resource including security controls. No scope restriction to backup resources only. |
| SP-006 | Entra ID | Service Principal | Copilot-SecurityAdvisor-Agent | Microsoft Copilot Studio security advisory agent | Security Team | 60 days ago | Today | None | Sites.Read.All + ChannelMessage.Read.All + Mail.Read + SecurityEvents.Read | Yes | No | Broad M365 read scope. No tool access restrictions, no escalation logic, no human override mechanism defined. Reads all Teams channels and mailboxes. |

### Microsoft Entra ID — OAuth Tokens (User-Delegated)

| NHI_ID | Source | Type | Name | Application/Purpose | Granted_By | Grant_Date | Last_Used | Expiration | Permission_Scope | Owner_Documented | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| OAUTH-001 | Entra ID | OAuth Token User-Delegated | ex-employee-docusign-token | DocuSign integration authorized by former employee | J.Martinez (termed) | 548 days ago | 312 days ago | None | Signature.ReadWrite + Documents.ReadWrite | No | No | Authorized by terminated employee. Token still active. DocuSign can still execute signatures on behalf of former employee identity. |
| OAUTH-002 | Entra ID | OAuth Token User-Delegated | slack-m365-integration | Slack Microsoft 365 connector authorized by department head | C.Thompson | 420 days ago | Today | None | Files.Read.All + Mail.Read + Calendars.Read | Yes | No | Files.Read.All scope — Slack integration can read all SharePoint files across organization via this delegated grant. |

### Google Workspace — Service Accounts

| NHI_ID | Source | Type | Name | Application/Purpose | Granted_By | Grant_Date | Last_Used | Expiration | Permission_Scope | Owner_Documented | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| GSA-001 | Google Workspace | Service Account | data-pipeline-sa@fec-prod.iam.gserviceaccount.com | BigQuery data pipeline for operational reporting | Data Engineering | 920 days ago | Today | None | BigQuery Admin + Storage Admin + Domain-Wide Delegation enabled | Yes | No | Domain-Wide Delegation enabled — this service account can impersonate any Google Workspace user in the organization. BigQuery Admin + Storage Admin provides full data access. |
| GSA-002 | Google Workspace | Service Account | looker-reporting-sa@fec-prod.iam.gserviceaccount.com | Looker BI reporting integration | Analytics Team | 540 days ago | Yesterday | None | BigQuery Data Viewer + Storage Object Viewer | Yes | Yes | Properly scoped read-only access. Monitored. No governance gaps identified. |
| GSA-003 | Google Workspace | Service Account | legacy-export-sa@fec-prod.iam.gserviceaccount.com | Legacy data export job — purpose unknown | Unknown | 1,460 days ago | 180 days ago | None | Storage Admin + Compute Admin | No | No | 4 years old. Owner unknown. Purpose undocumented. Last used 180 days ago. Compute Admin scope — can create, modify, or delete any GCP compute resource. Likely abandoned but still active. |

### Google Workspace — OAuth App Grants

| NHI_ID | Source | Type | Name | Application/Purpose | Granted_By | Grant_Date | Last_Used | Expiration | Permission_Scope | Owner_Documented | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| GOAUTH-001 | Google Workspace OAuth | OAuth App Grant | Zapier-WorkspaceIntegration | Zapier workflow automation | Multiple end users (17 individual grants) | Various (oldest 1,100 days) | Various | None | Gmail.modify + Drive.ReadWrite + Calendar.ReadWrite + Contacts.ReadWrite | No | No | 17 individual user grants — no central governance. Gmail.modify allows reading, sending, and deleting email on behalf of users. |
| GOAUTH-002 | Google Workspace OAuth | OAuth App Grant | Grammarly-WorkspacePlugin | Grammarly writing assistant | 43 end users | Various | Today | None | Gmail.readonly + Docs.readonly + Drive.readonly | No | No | 43 grants. Grammarly reads all Gmail and Drive content for AI processing. Data leaves Google Workspace to third-party AI training pipeline. No DLP policy applied. |

### SaaS Application API Keys

| NHI_ID | Source | Type | Name | Application/Purpose | Granted_By | Grant_Date | Last_Used | Expiration | Permission_Scope | Owner_Documented | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| API-001 | Salesforce | API Key | sf-integration-key-prod | Salesforce production API integration — internal CRM sync | Integration Team | Never rotated | Today | None | Full API access including data export | Yes | No | Full Salesforce API access. Never rotated since provisioning. No monitoring on API usage anomalies. |
| API-002 | ServiceNow | API Key | snow-automation-key | ServiceNow IT automation and ticket management | IT Ops | 547 days ago | Today | None | Admin API access including user provisioning and role assignment | Yes | No | Admin API scope — can provision accounts and assign roles via API. No usage monitoring. |
| API-003 | Slack | API Key | slack-secops-bot-key | Security operations Slack bot for alert notifications | Security Team | 90 days ago | Today | None | channels:write + chat:write + users:read + files:read | Yes | Yes | Appropriately scoped for purpose. Rotated. Monitored. No governance gaps. |
| API-004 | GitHub | API Key | github-legacy-pat | Legacy personal access token — purpose unknown | Unknown (personal token) | 1,200 days ago | Today | None | repo:admin + org:admin + delete_repo | No | No | Personal access token with org:admin scope — can modify organization settings, add/remove members, and delete repositories. Owner unknown. 1,200 days old. |

### Agentic AI Identities

| NHI_ID | Source | Type | Name | Application/Purpose | Granted_By | Grant_Date | Last_Used | Expiration | Permission_Scope | Owner_Documented | Monitored | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| AGENT-001 | Anthropic API | API Key | n8n-anthropic-prod-key | n8n OT/ICS threat intel pipeline production API key | Security Team | Never rotated | Today | None | LLM inference — processes all data passed to it including threat intel feed content | Yes | No | No blast radius definition. No output filtering. No human override mechanism. No data classification policy applied to inputs. |
| AGENT-002 | Microsoft | Service Principal | Copilot-Studio-SecurityAgent-SP | Copilot Studio security advisory agent service principal | Security Team | 60 days ago | Today | None | Sites.Read.All + ChannelMessage.Read.All + Mail.Read + SecurityEvents.Read | Yes | No | Reads all Teams channels, SharePoint sites, and mailboxes. No tool access restrictions. No escalation logic defined. No human override mechanism. |

---

## Ground Truth Validation Checklist

Use this table to validate nhi-risk-scorer.md output 
quality. Every Critical and High finding below must 
appear in the skill output. GSA-002 and API-003 must 
score Low — over-flagging these indicates a skill 
calibration issue.

| NHI ID | Planted Gap | Expected Score |
|---|---|---|
| SP-001 | Directory.Read.All scope creep, no monitoring | High |
| SP-002 | Files.ReadWrite.All, orphaned owner, no monitoring | Critical |
| SP-003 | RoleManagement.ReadWrite.Directory = effective Global Admin | Critical |
| SP-004 | Mail.Read with no justification, likely abandoned, unknown owner | High |
| SP-005 | Subscription Contributor — full Azure blast radius | Critical |
| SP-006 | Agentic identity, broad M365 scope, no guardrails | High |
| OAUTH-001 | Terminated employee token still active | Critical |
| OAUTH-002 | Files.Read.All via Slack delegated grant | High |
| GSA-001 | Domain-Wide Delegation + BigQuery Admin = full org data access | Critical |
| GSA-002 | No gaps — properly governed | Low |
| GSA-003 | Abandoned 4-year-old SA with Compute Admin scope | Critical |
| GOAUTH-001 | 17 ungoverned Zapier grants, Gmail.modify scope | High |
| GOAUTH-002 | 43 grants, data leaving Workspace to third-party AI | High |
| API-001 | Full Salesforce API, never rotated, no monitoring | High |
| API-002 | Admin API scope, ServiceNow user provisioning capability | High |
| API-003 | No gaps — properly governed | Low |
| API-004 | Abandoned PAT, org:admin scope, unknown owner | Critical |
| AGENT-001 | No blast radius, no output filtering, no human override | High |
| AGENT-002 | Broad M365 agentic scope, no tool restrictions | High |

Expected distribution: 7 Critical, 10 High, 2 Low.

---

## Version History

| Version | Date | Notes |
|---|---|---|
| 1.0 | April 2026 | Initial release — SaaS/IdP focused inventory |
