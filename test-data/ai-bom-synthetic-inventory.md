# Synthetic AI-BOM Test Inventory — ai-bom-inventory.md Validation

## Purpose
Synthetic AI component inventory for validating
ai-bom-inventory.md skill file output quality. Contains
deliberate governance gaps across all discovery tiers
and component types with known expected scores for
ground truth validation.

## Organization
Fictional Energy Co. (FEC) — Hybrid IT/OT Environment

## Discovery Tiers Simulated
- Tier 1: Entra ID export, Google Workspace OAuth report,
  Cloud AI usage, Endpoint management query (Intune),
  CASB report, DNS egress log
- Tier 2: Browser extension inventory, Code repository scan
- Tier 3: Vector database inventory, Knowledge base audit

## Suggested Parameters for Test Run
- ENVIRONMENT: hybrid
- COMPLIANCE_CONTEXT: NERC CIP
- SCOPE: all
- DISCOVERY_TIER: all

---

## Input Source 1 — Entra ID Enterprise Apps Export

| Component | Type | Registered | Last_Used | Permission_Scope | Owner | Notes |
|---|---|---|---|---|---|---|
| Microsoft 365 Copilot | Embedded Vendor AI | 180 days ago | Today | M365 tenant-wide | IT Admin | Deployed org-wide. No acceptable use policy published. No data handling documentation for sensitive content processing. |
| GitHub-Copilot-Enterprise | Embedded Vendor AI | 90 days ago | Today | Code repository read/write | DevOps Lead | Licensed. No policy on what data developers are permitted to submit to Copilot for completion. Source code containing internal credentials submitted by developers not governed. |
| PowerAutomate-AIBuilder-FinanceFlows | Automation / Workflow with AI Node | 270 days ago | Today | SharePoint read/write + Exchange read | Unknown | Owner left organization. 14 active flows using AI Builder document processing. Processing invoices and contracts — contains PII and financial data. |
| Copilot-OT-AdvisoryAgent | Agentic AI System | 60 days ago | Today | Sites.Read.All + SecurityEvents.Read + Teams.ReadWrite | Security Team | Agent deployed for OT security advisory. No blast radius definition. No human override mechanism. Reads all Teams channels and SharePoint sites. |
| Zapier-HRIntegration | Automation / Workflow with AI Node | 420 days ago | 3 days ago | Gmail.modify + Drive.ReadWrite + Contacts.ReadWrite | Unknown | Owner unknown. Processing HR documents including employee PII via Zapier AI actions. Gmail.modify scope — can read, send, delete email. |

---

## Input Source 2 — Google Workspace OAuth App Report

| Component | Type | Granted_By | Grant_Date | Last_Used | Permission_Scope | Owner | Notes |
|---|---|---|---|---|---|---|---|
| Grammarly-WorkspacePlugin | Embedded Vendor AI | 43 users (individual grants) | Various | Today | Gmail.readonly + Docs.readonly + Drive.readonly | No central owner | 43 individual grants. Grammarly processes all Gmail and Drive content via third-party AI pipeline. No DLP policy applied. Data leaves Google Workspace to Grammarly AI training infrastructure. |
| Notion-AIWorkspace | Embedded Vendor AI | 12 users | 180 days ago | Today | Drive.ReadWrite + Docs.ReadWrite | No central owner | Notion AI enabled. Processing internal documents. Outputs stored in Notion — access controls not matched to source Google Drive permissions. Ghost condition potential. |
| Make-AutomationPlatform | Automation / Workflow with AI Node | IT Ops | 365 days ago | Today | Gmail.read + Drive.read + Calendar.read | IT Ops | Make (formerly Integromat) workflows with AI modules processing operational data. 8 active scenarios. No inventory of which scenarios use AI modules vs standard automation. |

---

## Input Source 3 — Azure OpenAI Usage Report

| Component | Type | Consuming_Application | Model | Daily_Calls | Data_Classification | Owner | Notes |
|---|---|---|---|---|---|---|---|
| FEC-ThreatIntel-Pipeline | AI Data Pipeline | n8n cloud workflow | claude-sonnet-4-6 | 1 call/day | CISA feed data — public | Security Team | Documented. Governed. Low risk — public data only, output delivered to security team inbox. |
| FEC-InvoiceProcessing-Prod | AI Data Pipeline | Power Automate flow | gpt-4o | 340 calls/day | Financial data — confidential | Unknown | High volume. Financial data including vendor payment information and contract terms. Owner undocumented. No data handling policy. Output stored in SharePoint site with broader access than source invoices. Ghost condition. |
| Unknown-App-AzureOAI-01 | Unknown | Unidentified application | gpt-4-turbo | 1,247 calls/day | Unknown | Unknown | Application ID does not match any registered Entra ID app. 1,247 daily calls. Data classification unknown. This application is not in any known inventory. |

---

## Input Source 4 — Intune Endpoint Management Export

| Component | Type | Endpoint | Installed_Date | Version | Owner | Notes |
|---|---|---|---|---|---|---|
| GitHub Copilot (VS Code extension) | Embedded Vendor AI | 47 engineering workstations | Various | Current | DevOps Team | Licensed deployment. Governed. Acceptable use policy exists. Low risk. |
| Cursor | Developer-Built AI Integration | 8 engineering workstations | Various | Current | Individual developers | Individually installed. No IT approval. Submitting internal codebase to Cursor AI for completion — data handling policy unclear. |
| Ollama | Local Model | 3 engineering workstations | 45-90 days ago | Various | Unknown | Local model inference engine. No network egress — invisible to DNS and CASB. Models installed: llama3, codellama, mistral. No data handling documentation. Data processed is unauditable. Owner unknown. |
| Claude Desktop App | Embedded Vendor AI | 22 workstations across Security, Legal, Finance | Various | Current | Individual users | Individually installed across sensitive business functions including Legal and Finance. No acceptable use policy for desktop app usage. Sensitive client data and legal documents potentially submitted to Claude API. |
| LM Studio | Local Model | 1 engineering workstation | 30 days ago | Current | Unknown | Local model inference engine. Running mixtral-8x7b locally. No data handling documentation. Owner unknown. Invisible to all network-based discovery. |

---

## Input Source 5 — CASB Report (Netskope — 90 Day)

| Component | Type | Users | Access_Frequency | Data_Uploaded_MB | Risk_Category | Notes |
|---|---|---|---|---|---|---|
| ChatGPT (chat.openai.com) | Foundation Model | 67 unique users | Daily | 2,340 MB over 90 days | Shadow AI | Not approved. 67 users submitting data directly to ChatGPT. 2,340 MB uploaded over 90 days — data classification unknown. No DLP policy blocking sensitive data upload. |
| Perplexity AI | Foundation Model | 23 unique users | Weekly | 140 MB over 90 days | Shadow AI | Not approved. Research queries — lower data volume but unknown data classification. |
| Claude.ai (claude.ai) | Foundation Model | 31 unique users | Daily | 890 MB over 90 days | Shadow AI | Not approved as standalone tool separate from Claude Desktop App inventory. 31 users. 890 MB uploaded — includes unknown data types. Separate from managed Claude Desktop App deployment. |
| Midjourney | Foundation Model | 4 unique users | Monthly | 12 MB over 90 days | Shadow AI | Not approved. Low volume. Image generation — lower data risk but policy gap. |

---

## Input Source 6 — DNS Egress Log (30 Day Filtered for AI Endpoints)

| Source_IP | Subnet | AI_Endpoint | Call_Count_30d | Matched_Known_Inventory | Notes |
|---|---|---|---|---|---|
| 10.22.45.18 | Engineering workstation pool | api.openai.com | 847 | No | No known approved application on this subnet making direct OpenAI API calls. Developer personal API key usage suspected. |
| 10.33.12.4 | Finance subnet | api.openai.com | 312 | No | No AI tool approved for Finance subnet. Direct API calls from Finance workstation — data classification unknown. High priority. |
| 10.44.21.9 | OT DMZ | api.anthropic.com | 23 | No | OT DMZ subnet making calls to Anthropic API. No AI tool approved for OT DMZ. 23 calls in 30 days — low volume but critical environment. Immediate investigation required. |
| 10.11.8.22 | IT infrastructure | api.anthropic.com | 31 | Yes — FEC-ThreatIntel-Pipeline | Matches known n8n threat intel pipeline. Expected. |
| 10.55.67.3 | HR subnet | generativelanguage.googleapis.com | 156 | No | HR subnet calling Google Gemini API directly. No approved tool. HR data — PII exposure risk. |

---

## Input Source 7 — Code Repository Scan (GitHub Enterprise)

| Repository | AI_Import_Found | API_Key_Pattern_Found | Data_Scope | Owner | Notes |
|---|---|---|---|---|---|
| fec-scada-analytics | import openai, langchain | OPENAI_API_KEY in .env file | SCADA telemetry data — OT Level 2 | Engineering Team | SCADA telemetry being processed by OpenAI API. OT data leaving the environment. .env file with API key committed to repository — credential exposure. CIP-010-4 and CIP-013-2 implications. |
| fec-hr-automation | from anthropic import Anthropic | ANTHROPIC_API_KEY in config.py | HR records — PII | HR Systems Team | HR PII being processed via Anthropic API. API key in plaintext in config file. No data handling documentation. |
| fec-finance-reporting | import openai | None found | Financial reporting data | Finance Team | Financial data processed via OpenAI API. No hardcoded key — using environment variable. Governed API key usage unclear. No data handling documentation. |
| fec-internal-tools | huggingface_hub imports | HUGGINGFACE_TOKEN in multiple files | Internal documentation | Unknown developer | HuggingFace model usage in internal tooling. Multiple token instances in multiple files — likely copy-pasted. Owner unknown. |

---

## Input Source 8 — Vector Database and Knowledge Base Audit

| Component | Type | Platform | Source_Data | Access_Controls | Source_Access_Status | Notes |
|---|---|---|---|---|---|---|
| FEC-SecurityKB-VectorStore | Derived Artifact Store | Azure AI Search (vector index) | Security advisories, incident reports, internal runbooks | Security team only | Source documents: mixed — some restricted post-ingestion | Vector embeddings of security documents. Some source documents have had access restricted since ingestion. Embeddings remain queryable by any system with Azure AI Search access. Ghost condition confirmed. |
| FEC-Contracts-NotionAI | Derived Artifact Store | Notion AI knowledge base | Legal contracts, vendor agreements | Notion workspace — broader than source SharePoint | Source in SharePoint with restricted access | Notion AI summaries of legal contracts stored in Notion workspace. Notion access controls broader than source SharePoint library. Any Notion workspace member can query AI summaries of restricted contracts. Ghost condition confirmed. |
| FEC-OpsReports-Confluence | Derived Artifact Store | Confluence (AI-generated summaries) | Operational reports including OT incident data | Confluence space — IT and Operations | Source reports: restricted to OT team | AI-generated summaries of OT incident reports stored in Confluence. Confluence space accessible to broader IT team. OT incident data accessible beyond authorized OT team members. Ghost condition confirmed. CIP-010-4 implication. |

---

## Input Source 9 — Business Function Interview Notes

| Function | AI_Tools_Disclosed | Built_Anything | Piloting | Notes |
|---|---|---|---|---|
| Security | Claude Desktop, n8n pipeline, Copilot Studio agent | n8n OT threat intel pipeline | Evaluating RAPTOR for vuln discovery | All disclosed tools already in inventory. |
| Legal | Claude Desktop, ChatGPT (personal account) | None disclosed | None | Using personal ChatGPT account for contract review — not corporate account. No DLP controls on personal account usage. Client contract data potentially submitted. |
| Finance | Claude Desktop, Excel Copilot | None disclosed | Evaluating AI-powered FP&A tool (unnamed) | Unnamed FP&A tool not in any inventory. Follow-up required. |
| HR | Grammarly, ChatGPT | Zapier workflow for onboarding automation | None | Zapier onboarding workflow not in known inventory — separate from Zapier-HRIntegration in Entra ID. Second ungoverned Zapier workflow confirmed. |
| Engineering | Cursor, GitHub Copilot, Ollama | fec-scada-analytics pipeline, fec-hr-automation | Evaluating Codeium | scada-analytics and hr-automation confirmed via code scan. Codeium not yet installed — monitor. |
| OT Operations | None disclosed | None disclosed | None | No AI tools disclosed. DNS log shows 10.44.21.9 (OT DMZ) making Anthropic API calls — not disclosed in interview. Discrepancy requires investigation. |

---

## Ground Truth Validation Checklist

| Component | Discovery Source | Expected Score | Key Finding |
|---|---|---|---|
| Unknown-App-AzureOAI-01 | Azure OpenAI usage | Critical | 1,247 daily API calls from unidentified application — not in any inventory |
| fec-scada-analytics repo | Code scan | Critical | OT/SCADA data sent to OpenAI API + API key committed to repo — CIP-010-4, CIP-013-2 |
| OT DMZ DNS hit (10.44.21.9) | DNS egress | Critical | OT DMZ making Anthropic API calls — not disclosed in interview, not in inventory |
| PowerAutomate-AIBuilder-FinanceFlows | Entra ID | Critical | Orphaned owner, PII/financial data, ghost condition on SharePoint output |
| Ollama (3 workstations) | Intune | Critical | Local models, no data handling docs, unauditable, owner unknown |
| FEC-InvoiceProcessing-Prod | Azure OpenAI usage | Critical | Financial data, unknown owner, ghost condition confirmed |
| Zapier-HRIntegration | Entra ID | Critical | Orphaned owner, PII processing, Gmail.modify scope |
| fec-hr-automation repo | Code scan | Critical | HR PII via Anthropic API, plaintext API key in config.py |
| FEC-OpsReports-Confluence | KB audit | Critical | OT incident data accessible beyond OT team — CIP-010-4 |
| Copilot-OT-AdvisoryAgent | Entra ID | Critical | Agentic, no blast radius, no human override, OT-adjacent scope |
| ChatGPT (CASB) | CASB | High | 67 users, 2,340 MB uploaded, no DLP, shadow AI |
| Claude.ai (CASB) | CASB | High | 31 users, 890 MB uploaded, unapproved standalone usage |
| Finance DNS hit (10.33.12.4) | DNS egress | High | Finance subnet API calls, no approved tool, PII risk |
| HR DNS hit (10.55.67.3) | DNS egress | High | HR subnet calling Gemini API, PII exposure |
| Engineering DNS hit (10.22.45.18) | DNS egress | High | Ungoverned personal API key usage suspected |
| FEC-SecurityKB-VectorStore | Vector DB audit | High | Ghost condition — restricted source docs, embeddings still queryable |
| FEC-Contracts-NotionAI | KB audit | High | Ghost condition — restricted contracts accessible via Notion AI summaries |
| Microsoft 365 Copilot | Entra ID | High | No AUP, no data handling documentation for sensitive content |
| Claude Desktop (Legal/Finance) | Intune | High | Sensitive functions, no AUP, client data risk |
| LM Studio (1 workstation) | Intune | High | Local model, no data handling docs, owner unknown |
| fec-finance-reporting repo | Code scan | High | Financial data via OpenAI API, no data handling documentation |
| Legal ChatGPT (personal) | Interview | High | Personal account usage for contract review — no corporate DLP controls |
| HR Zapier onboarding | Interview | High | Second ungoverned Zapier workflow — not in Entra ID inventory |
| fec-internal-tools repo | Code scan | High | HuggingFace tokens copy-pasted across files, owner unknown |
| Grammarly (Google Workspace) | Google OAuth | High | 43 grants, data to third-party AI, no DLP |
| Notion-AIWorkspace | Google OAuth | High | Ghost condition potential, access control mismatch |
| GitHub Copilot Enterprise | Entra ID | Medium | No policy on sensitive data submission — governance gap not Critical |
| Cursor (8 workstations) | Intune | Medium | No IT approval, data handling unclear — not Critical as no OT access |
| Make-AutomationPlatform | Google OAuth | Medium | No AI module inventory — documentation gap |
| Perplexity AI (CASB) | CASB | Medium | 23 users, shadow AI, lower data volume |
| FEC-ThreatIntel-Pipeline | Azure OpenAI usage | Low | Documented, governed, public data only |
| GitHub Copilot VS Code | Intune | Low | Licensed, governed, AUP exists |
| Midjourney (CASB) | CASB | Low | 4 users, image generation, minimal data risk |

Expected distribution: 10 Critical, 17 High, 4 Medium, 3 Low.

Self-inventory entry AI-BOM-000 must appear in output
representing the Claude model executing the assessment.

---

## Version History

| Version | Date | Notes |
|---|---|---|
| 1.0 | April 2026 | Initial release — multi-tier discovery across hybrid IT/OT environment |
