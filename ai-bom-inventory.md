# Skill: AI Bill of Materials (AI-BOM) Asset Inventory

## Version
1.1 — April 2026

## Author
Mike Poulos, Executive Advisor — Cybersecurity
Windval Technology Solutions

## Purpose
Discover, classify, and assess governance posture of all
AI components in an organization's environment — including
unknown and ungoverned components. Produces a structured
AI Bill of Materials with risk classifications, data
lineage flags, governance gap analysis, and framework
mappings.

## Background
AI dramatically lowers the barrier to building new tools.
It does not lower the barrier to understanding what already
exists. Most organizations have deployed more AI components
than they have cataloged — through vendor-embedded AI
features, developer-built integrations, citizen-developer
automations, and individual employee tool adoption —
none of which went through IT governance.

The result is an environment where AI components ingest
sensitive data, produce derived outputs stored in unknown
locations, operate under ungoverned identities, and
create undocumented blast radius exposure.
Known AI inventory is table stakes. Unknown AI discovery
is where the real security exposure lives.

This skill encodes practitioner judgment for discovering,
classifying, and assessing AI components across
known and unknown layers. It handles structured inventory
inputs and unstructured signal data — DNS logs, CASB
reports, code repository scans, endpoint management
exports, and interview notes — extracting and normalizing
AI component inventory from whatever data is available.

Note: This skill must include itself as an inventory
entry. The AI agent executing this assessment is an AI
component operating in the client environment during
the engagement. It must appear in the final inventory
as entry AI-BOM-000. Failure to self-inventory is the
canonical AI governance failure — do not replicate it.

## Parameters

- **INPUT:** One or more of the following — structured
  AI tool inventory, Entra ID / Google Workspace app
  export, cloud resource inventory, CASB / SSE AI
  category report, DNS egress log filtered for AI
  endpoints, endpoint management platform export
  (Intune / SCCM / Jamf), code repository scan
  results, DLP log excerpts, vector database
  inventory, procurement records, business function
  interview notes. Partial input is the normal
  starting condition — gaps in input are findings.
- **ENVIRONMENT:** cloud / OT / hybrid / enterprise / all
- **COMPLIANCE_CONTEXT:** EU AI Act / NERC CIP /
  NIST AI RMF / SOC2 / FedRAMP / none
- **DISCOVERY_TIER:**
  - Tier 1 (passive signal — DNS, CASB, IdP exports,
    endpoint management query)
  - Tier 2 (endpoint and code — EDR telemetry,
    browser extension inventory, repository scan)
  - Tier 3 (data lineage — vector stores, DLP,
    derived artifact mapping)
  - all
- **SCOPE:** models / agents / automations /
  vendor-embedded / developer-built /
  citizen-developer / data-pipelines / all

## Input Source Guidance

### Known AI Discovery Sources
Structured inputs that surface governed or partially
governed AI components:

- **Entra ID Enterprise Apps / App Registrations:**
  Every OAuth app, service principal, and API
  integration registered in the M365 tenant. Highest
  yield single source for SaaS AI tools and agent
  identities.
- **Google Workspace OAuth App Access Report:**
  Every third-party app granted access to Gmail,
  Drive, Calendar, or Meet. Surfaces consumer AI
  tools adopted without IT approval.
- **Cloud AI Service Usage Reports:** Azure OpenAI
  usage logs, AWS Bedrock invocation metrics, GCP
  Vertex AI usage — shows which models are being
  called, by which applications, at what volume.
- **Copilot Studio Admin Center:** Every agent built
  in the M365 tenant, owner, data connections,
  user interaction volume.
- **Power Automate Admin Center:** Every flow
  including AI Builder flows using document
  processing, prediction, or form recognition.
- **SaaS Procurement / AP Records:** Vendor contracts
  and purchase records filtered for AI, ML,
  automation, intelligence, or agent terminology.
- **IT Asset Register / CMDB:** Any AI-tagged or
  automation-tagged assets in existing inventory
  systems.
- **Endpoint Management Platform Query (Intune /
  SCCM / Jamf / Software Center):** Query Desktop
  Engineering or IT Operations for installed
  application inventory filtered for known AI
  components across managed endpoints. Sources
  include Microsoft Intune device compliance and
  app inventory reports, SCCM software inventory
  queries, Jamf Pro inventory for macOS fleets,
  and Software Center deployment records.
  Filter for: GitHub Copilot, Cursor, Codeium,
  Tabnine, Continue, Cline, Amazon Q Developer,
  ChatGPT desktop app, Claude desktop app,
  Perplexity desktop app, Ollama, LM Studio,
  and any locally-running model inference engines.
  Locally-running models (Ollama, LM Studio) are
  high-priority findings — they process data
  entirely on-device with no network egress to
  known AI endpoints, making them invisible to
  DNS and CASB discovery. They will not appear
  in any network-based signal source.

### Unknown AI Discovery Sources
Signal data requiring interpretation to extract
AI component inventory:

- **DNS Egress Logs:** Filter for known AI API
  endpoints including api.openai.com,
  api.anthropic.com,
  generativelanguage.googleapis.com,
  bedrock-runtime.*.amazonaws.com,
  *.openai.azure.com, huggingface.co,
  api.cohere.ai, api.mistral.ai, together.ai,
  groq.com. Each source IP hitting these endpoints
  is an AI component — correlate against known
  inventory. Unmatched source IPs are unknown
  AI components requiring investigation.
- **CASB / SSE AI Category Reports:** Netskope,
  Zscaler, Palo Alto Prisma Access AI app category
  reports. Every AI tool accessed from the corporate
  network in the past 30-90 days regardless of IT
  awareness.
- **Browser Extension Inventory:** EDR or endpoint
  management (Intune, Jamf) enumeration of installed
  browser extensions filtered for known AI tools —
  ChatGPT, Claude, Perplexity, Grammarly, Copilot,
  and similar. Distinct from installed application
  inventory — a user may have the Claude browser
  extension without the Claude desktop app, or
  vice versa. Both must be assessed.
- **Code Repository Scan Results:** GitHub, Azure
  DevOps, GitLab, or Bitbucket repositories scanned
  for AI library imports (openai, anthropic,
  langchain, llama_index, transformers, torch,
  huggingface_hub) and AI API key environment
  variables (OPENAI_API_KEY, ANTHROPIC_API_KEY,
  COHERE_API_KEY, HUGGINGFACE_TOKEN). Each matching
  repository contains a developer-built AI component.
- **DLP Logs:** Data transmissions to AI API
  endpoints. Each instance is both an unknown AI
  usage event and a potential data exposure event.
  Cross-reference against known inventory — unmatched
  transmissions indicate ungoverned AI data flows.
- **Business Function Interview Notes:** Responses
  to three questions asked of each business function
  lead — what AI tools does your team use daily,
  has anyone on your team built automations or
  agents using tools like Copilot Studio, Power
  Automate, Zapier, Make, or n8n, and what AI
  tools are being piloted that IT may not know
  about yet. Target Security, HR, Finance, Legal,
  Operations, and Engineering leads minimum.

### Derived Artifact Sources
Sources that reveal the ghost layer — AI-generated
content that has propagated beyond governed
boundaries:

- **Vector Database Inventory:** Pinecone, Weaviate,
  Chroma, pgvector, Azure AI Search with vector
  index. Each vector database is a derived artifact
  store containing embeddings generated from source
  documents. If source access was revoked after
  embeddings were generated, the embeddings remain
  — restricted source, ghost still talking.
- **Knowledge Base and Wiki Exports:** Confluence,
  SharePoint, Notion, and similar platforms used
  to store AI-generated summaries, reports, or
  reference material. These are derived artifact
  repositories with independent access controls
  that may not match source document controls.
- **LLM Output Caches:** Any system storing AI
  model outputs for reuse — RAG pipeline caches,
  summarization stores, automated report archives.
  Assess whether cached outputs are governed by
  the same access controls as the source data
  that generated them.

## Process

### Step 1 — Normalize and Deduplicate Input
Before classification, process all input sources:

- Extract AI component references from each input
  source regardless of format — structured table,
  log excerpt, interview note, or scan output
- Assign a temporary source tag to each extracted
  component (DNS, CASB, IdP, EndpointMgmt,
  CodeScan, Interview, DLP, etc.)
- Deduplicate across sources — the same tool
  appearing in DNS logs, a CASB report, an endpoint
  management export, and an interview note is one
  component with four discovery signals, not four
  components. More discovery signals increase
  classification confidence.
- **Cross-Source Correlation for Unknown Components:**
  When an unknown component is identified via DNS or
  CASB with no matching known inventory entry, cross-
  reference against all other discovery sources before
  treating it as a new unknown component. A DNS hit
  from an Engineering workstation pool with no matching
  known application should be cross-referenced against
  the endpoint management export for developer AI tools
  installed on that subnet. A match reduces the finding
  from unknown to ungoverned — still a governance gap
  but a different remediation path. An unresolved
  cross-reference after checking all sources confirms
  a genuinely unknown component requiring immediate
  investigation.
- Flag components appearing in only one source
  as lower confidence — corroboration across
  multiple sources increases classification
  accuracy
- Add self-inventory entry AI-BOM-000 for the
  AI agent executing this assessment before
  processing any other entries

### Step 2 — Classify Each AI Component by Type
Assign each component to exactly one primary type:

- **Foundation Model:** Third-party LLM or AI
  model accessed via API — OpenAI GPT series,
  Anthropic Claude, Google Gemini, Meta Llama,
  Mistral, Cohere, and similar
- **Local Model:** AI model running entirely
  on-device via local inference engine — Ollama,
  LM Studio, GPT4All, or similar. No network
  egress. Invisible to DNS and CASB. Only
  detectable via endpoint management query.
  Flag all local models as requiring immediate
  data handling assessment — there is no
  visibility into what data these models process.
- **Embedded Vendor AI:** AI features within
  existing SaaS products — Microsoft 365 Copilot,
  Salesforce Einstein, ServiceNow AI, GitHub
  Copilot, Workday AI, Grammarly, and similar.
  Client may not have consciously adopted these —
  they shipped inside tools already in use.
- **Agentic AI System:** Autonomous or
  semi-autonomous agent that plans, reasons, and
  takes actions — Copilot Studio agents, n8n
  pipelines with AI nodes, LangChain agents,
  AutoGPT instances, custom-built agents using
  any agent framework
- **Automation / Workflow with AI Node:** Workflow
  automation containing one or more AI steps but
  not fully autonomous — Power Automate flows with
  AI Builder, Zapier workflows with AI actions,
  Make scenarios with AI modules
- **Developer-Built AI Integration:** Custom code
  calling AI APIs directly — applications,
  scripts, notebooks, or microservices with direct
  model API calls identified via code repository
  scan
- **AI Data Pipeline:** Automated data processing
  using AI for transformation, classification,
  summarization, or enrichment — includes RAG
  pipelines, embedding generation jobs, and
  automated summarization workflows
- **Derived Artifact Store:** Repository of
  AI-generated outputs — vector databases,
  knowledge bases, automated report archives,
  LLM output caches

### Step 3 — Assess Each Component Against
### Six Governance Dimensions

For each component evaluate:

- **Ownership:** Named owner with active
  accountability — no owner is Critical gap
- **Purpose Documentation:** Stated business
  purpose, use case, and authorized data scope
  documented — undocumented is High gap
- **Data Inventory:** What data does this
  component ingest, what does it produce, where
  does the output live, is output governed by
  the same access controls as input — ungoverned
  data flow is High gap. For local models,
  absence of any data handling documentation
  is Critical gap.
- **Blast Radius Definition:** If this component
  behaves unexpectedly or is compromised, what
  is the maximum impact — undefined blast radius
  on any agentic component is Critical gap
- **Adversarial Signal Assessment:** For any unknown
  component discovered via DNS or CASB in a critical
  network segment (OT DMZ, ICS network, production
  infrastructure), assess whether the AI API calls
  could represent adversarial activity rather than
  ungoverned tool usage. An unknown process making
  API calls to external LLM endpoints from an OT
  DMZ subnet is equally consistent with an
  AI-assisted command and control channel as with
  an ungoverned tool deployment. Treat OT network
  AI API egress as a potential security incident
  requiring investigation, not just a governance
  gap requiring remediation. Escalate to the OT
  security team immediately — do not treat as
  routine inventory finding.
- **Human Override Mechanism:** For agentic
  components, is there a defined human
  intervention point — no override mechanism
  on any autonomous component is Critical gap
- **Monitoring and Logging:** Is component
  activity logged, are anomalies alerted, is
  usage reviewed — no monitoring is High gap.
  Local models have no inherent logging — absence
  of compensating endpoint monitoring controls
  is Critical gap.

### Step 4 — Flag Derived Artifact Risk
For each component that produces output stored
elsewhere:

- Identify where outputs are stored
- Assess whether output access controls match
  input access controls
- Flag any case where input was restricted
  after output was generated — this is the ghost
  condition: restricted source, output still
  accessible
- Assess whether outputs are indexed by other
  AI components that could propagate them
  further — ghost depth assessment. Each
  additional hop is a ghost of a ghost.
- Flag vector databases and knowledge bases
  as requiring explicit data lineage
  documentation regardless of other governance
  posture

### Step 5 — Score Governance Risk
Assign a governance risk score to each component:

| Score | Label | Criteria |
|---|---|---|
| 1 | Critical | No owner + undefined blast radius + no monitoring OR agentic component with no human override OR local model with no data handling documentation |
| 2 | High | One or more High governance gaps present OR unknown component with no inventory corroboration |
| 3 | Medium | Minor governance gaps — documentation or monitoring gaps only |
| 4 | Low | Fully governed — owner, purpose, data inventory, blast radius, monitoring all documented |

Elevate any component to Critical regardless of
other factors if:
- It has access to OT/ICS environments or
  BES Cyber Systems
- It processes regulated data (PII, PHI, NERC
  CIP protected information) without documented
  data handling controls
- It is an agentic component with no blast
  radius definition and no human override
- It is a derived artifact store whose access
  controls do not match source document controls
- It is a local model with no data handling
  documentation — data processed by local
  models is unauditable by default

Score unknown components — those discovered via
DNS, CASB, code scan, endpoint management, or
interview with no corroborating inventory entry
— as High minimum. Unknown presence is itself
a governance gap.

### Step 6 — Map to Compliance Frameworks
Map each Critical and High finding to applicable
controls:

**EU AI Act (August 2026):**
- Article 9 (Risk management system)
- Article 10 (Data governance)
- Article 13 (Transparency and documentation)
- Article 17 (Quality management)

**NERC CIP:**
- CIP-005-7 (Electronic Security Perimeter —
  AI components with OT access)
- CIP-007-6 (Systems Security — ungoverned
  AI tool as unauthorized system)
- CIP-010-4 (Configuration Change Management —
  undocumented AI component as config gap)
- CIP-013-2 (Supply Chain — vendor-embedded
  AI and third-party model dependencies)

**NIST AI RMF:**
- GOVERN 1 (Policies and accountability)
- MAP 1 (AI context and categorization)
- MEASURE 2 (Risk evaluation)
- MANAGE 1 (Risk treatment)

**NIST CSF 2.0:**
- GV.OC (Organizational context)
- GV.SC (Supply chain risk — vendor AI features)
- ID.AM (Asset management — AI component inventory)
- DE.CM (Continuous monitoring — AI activity)

**MITRE ATLAS:**
- AML.T0040 (ML Inference API Access)
- AML.T0047 (ML-Enabled Product Abuse)
- AML.T0018 (Backdoor ML Model — supply chain)

**OWASP LLM Top 10 2025:**
- LLM06 (Excessive Agency)
- LLM02 (Sensitive Information Disclosure)
- LLM09 (Misinformation — ungoverned output)

**OWASP Agentic Top 10 2026:**
- ASI01 (Agent Goal Hijack)
- ASI02 (Tool Misuse and Exploitation)
- ASI03 (Identity and Privilege Abuse)
- ASI04 (Agentic Supply Chain Vulnerabilities)
- ASI08 (Cascading Failures — multi-agent
  blast radius)

**SOC2:**
- CC6.1 (Logical access — AI component access)
- CC7.1 (System monitoring)
- CC9.2 (Vendor risk — third-party AI)

### Step 7 — Produce AI-BOM Output
Structured inventory and risk output ordered
Critical first.

## Output Format

Write in crisp direct declarative sentences. No
marketing language. No filler, minimal narrative.
State findings and actions — nothing else.

### Section 1 — Executive Summary
Three sentences maximum. State total AI component
count by discovery method (known vs. unknown),
count by risk tier, and the single highest-priority
finding with its blast radius or data exposure
implication.

### Section 2 — AI-BOM Inventory Table
Complete structured inventory ordered Critical first:

| Component ID | Name | Type | Discovery Method | Owner | Data Ingested | Output Location | Risk | Primary Gap | Urgency |

### Section 3 — Unknown Component Findings
Separate section for every component discovered
via DNS, CASB, code scan, endpoint management
query, or interview that does not appear in any
known inventory source. Each entry states the
discovery signal, the source IP or repository or
endpoint, confidence level, and recommended
immediate action.

### Section 4 — Derived Artifact Risk
Separate section for every identified ghost
condition — components whose outputs persist
beyond source access controls. State the source
component, the output location, the access
control mismatch, and ghost depth if assessable.

### Section 5 — Local Model Findings
Separate section for any locally-running model
instances identified via endpoint management
query. State the endpoint, the model, the
installed date if available, the owner if
determinable, and the data handling gap.
Local models require immediate data handling
assessment regardless of other governance posture.

### Section 6 — Governance Gap Summary
One paragraph per governance dimension (ownership,
purpose documentation, data inventory, blast
radius, human override, monitoring). Identify
systemic gaps vs. isolated issues.

### Section 7 — Self-Inventory Entry
Confirm that AI-BOM-000 appears in the inventory
table representing the AI agent that executed
this assessment. State the model used, the data
processed during the engagement, and confirm no
client data is retained beyond the session.

## Quality Benchmark
A high-quality output identifies components the
client did not know existed, names specific data
flows and output locations, and produces
remediation actions specific enough to assign
to an engineer or team. Crisp direct declarative sentences. 
No marketing language. No filler, minimal narrative. 

Poor output:
"Several unknown AI tools were identified via
network logs. Governance should be improved."

Good output:
"DNS egress logs show 14 source IPs making calls
to api.openai.com that do not correspond to any
known AI tool in the inventory. Source IP
10.22.45.18 (Engineering workstation pool)
accounts for 847 API calls in the past 30 days.
Source IP 10.33.12.4 (Finance subnet) accounts
for 312 calls — no AI tool is approved for use
in Finance. Endpoint management query identified
Ollama installed on 3 engineering workstations
with no data handling documentation — data
processed by these local models is unauditable.
Recommend immediate firewall rule blocking direct
AI API egress except from approved source IPs,
mandatory IT review of all Finance AI tool
requests, and immediate data handling assessment
for all Ollama installations. CIP-007-6, ID.AM,
ASI04, Article 10."

## Known Limitations
- Unknown AI discovery is inherently incomplete —
  components with no network egress, no code
  repository presence, no endpoint management
  visibility, and no interview disclosure will
  not be found by any discovery method
- Vendor-embedded AI features change without
  notification — an AI-BOM has a shelf life and
  requires periodic refresh as vendors update
  their products
- Derived artifact depth is difficult to assess
  without access to each downstream system —
  ghost chains beyond two hops are typically
  undetectable without dedicated data lineage
  tooling
- Code repository scan accuracy depends on
  repository access permissions — private
  repositories not accessible to the scanning
  tool will not be assessed
- Locally-running AI models (Ollama, LM Studio,
  and similar local inference engines) are
  invisible to all network-based discovery
  methods — DNS, CASB, DLP, and firewall logs
  will show no egress. Endpoint management
  platform query is the only reliable detection
  method. Environments without centralized
  endpoint management have no visibility into
  local model usage.
- NERC CIP applicability requires BES Cyber
  Asset classification — assumes classification
  is provided or inferable from input
- This skill processes client data including
  potentially sensitive inventory information,
  log excerpts, and interview notes — handle
  all inputs in accordance with client data
  handling requirements and do not retain
  beyond the engagement session

## Related Skills
- nhi-risk-scorer.md (identity risk assessment
  for AI components surfaced by this skill)
- soc-decision-audit.md (audit trail for
  AI-BOM risk decisions)
- nerc-cip-gap-analysis.md (CIP compliance
  gap analysis for OT-adjacent AI components)

## Version History
| Version | Date | Change |
|---|---|---|
| 1.0 | April 2026 | Initial release |
| 1.1 | April 2026 | Added adversarial signal assessment 
for OT network AI egress and cross-source correlation 
guidance — findings from validation run against 
synthetic inventory |
