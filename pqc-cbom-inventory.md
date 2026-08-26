---
name: pqc-cbom-inventory
description: >-
  Enable on-demand compilation of a Post-Quantum Cryptography
  Cryptographic Bill of Materials (PQC-CBOM) from source
  technology extracts across IT, cloud, applications, identity,
  network, PKI, OT/ICS, and third-party services. The Microsoft
  365 Copilot agent ingests current flat-file or connected
  extracts from network crypto scanners, PKI/CA, AppSec/SCA/SAST,
  IAM, HSM/KMS, cloud crypto services, OT/ICS inventories, and
  vendor roadmaps, then applies classification, multi-source
  reconciliation, quantum-vulnerable analysis, ownership mapping,
  and prioritization logic at query time. Use when establishing
  a low-overhead living cryptographic inventory capability that
  avoids a separately maintained structured CBOM. Accepts
  structured and unstructured source technology extracts.
  Treats source extracts as the system of record; the agent is
  the compilation and query engine.
license: MIT
compatibility: >-
  Designed for Microsoft 365 Copilot and adaptable to other
  Agent Skills compatible clients. Microsoft 365 enterprise
  integrations are optional; other clients must be provided
  equivalent source technology extracts. Can operate entirely
  on provided files. Connected-source operation requires
  authorized enterprise integrations. No local system packages
  are required.
metadata:
  author: mike-poulos
  organization: Windval Technology Solutions
  version: "1.7"
  domain: cybersecurity
  subdomain: post-quantum-cryptography
  validated: "false"
  validation-score: pending
  standards-alignment: >-
    FIPS 203, FIPS 204, FIPS 205, NIST PQC Migration Project,
    CNSA 2.0 where applicable
  supporting-references: >-
    NIST SP 800-208, NIST IR 8413
---
# Skill: PQC Cryptographic Bill of Materials (PQC-CBOM)

## Version
1.7 — August 2026

## Author
Mike Poulos, Executive Advisor — Cybersecurity
Windval Technology Solutions

## Purpose
Enable a low-overhead, living PQC-CBOM capability by treating
source technology extracts as the authoritative sources of
truth and using a Microsoft 365 Copilot agent as the
compilation and query engine.

The agent ingests current flat-file or connected extracts
from the relevant cryptographic discovery sources, applies
the Discovery Coverage Matrix, asset-class classification,
multi-source reconciliation, quantum-vulnerable analysis,
ownership mapping, and prioritization logic, and produces
on-demand CBOM views, delta views, and natural-language
answers.

No separately maintained structured CBOM inventory is required.
Freshness is determined solely by the currency of the source
technology extracts. When current source extracts are made
available, the next agent run reflects them.

The compiled PQC-CBOM view is a derived product. Source
extracts and their evidence references remain authoritative.
Material findings must retain traceability to the originating
record.

Workflow order remains:

> Source extracts first. Validate and reconcile second.
> Prioritize third. Enable continuous query and delta fourth.

v1.7 does not add new analysis logic. It enforces the v1.6
output contract deterministically: schema completeness,
Manifest completeness, count integrity, and the Pre-Output
Validation gate. Validation failures must be corrected before
responding.

## Background
A materialized, structured CBOM that depends on ongoing human
or process discipline to stay current frequently becomes another
stale inventory. That is the classic failure mode of point-in-time
assessments and many SBOM programs.

Living capability is achieved more sustainably by:
- Keeping the source technology extracts (scans, CA exports,
  SCA results, HSM inventories, cloud crypto exports, OT
  inventories, vendor roadmaps) current, and
- Using a Microsoft 365 Copilot agent to compile, reconcile,
  classify, and query the CBOM view on demand when current
  source extracts are made available.

This model eliminates the second system of record, reduces
ongoing overhead, and keeps the cryptographic inventory as
fresh as the underlying discovery sources.

## Parameters

- **INPUT:** Current source technology extracts from one or
  more of the following — network cryptographic scan output,
  certificate inventory or CA export, application dependency /
  SCA / SAST results, HSM/KMS inventory, cloud cryptographic
  service export, IAM/federation configuration inventory,
  OT/ICS asset or architecture inventory, vendor product
  inventory or roadmap evidence, CMDB extracts. Prior source
  snapshots may be supplied for delta analysis.
- **ENVIRONMENT:** enterprise / cloud / network / application /
  identity / PKI / OT/ICS / hybrid / all
- **SCOPE:** Technology domain, business unit, environment,
  application portfolio, or asset group
- **DISCOVERY_DEPTH:** baseline / validated / reconciled
- **MODE:** initial | delta | continuous / on-demand
- **OUTPUT_FORMAT:** markdown / CSV / JSON / XLSX-ready table /
  natural-language agent response
- **EVIDENCE_STANDARD:** observed / authoritative /
  vendor-attested / owner-validated
- **PQC_CONTEXT:** NIST / CNSA 2.0 / contractual /
  internal policy / general readiness

## Process

### Step 1 — Establish Scope Using Discovery Coverage Matrix
Define business services, technology domains, environments,
and asset populations in scope. Identify data classifications
and confidentiality shelf-life requirements where available.
Use the Discovery Coverage Matrix to identify expected
Primary and Strong sources for each technology area and the
extract formats the agent will consume.

| Technology / Discovery Area | Tenable | PKI / CA | AppSec / SCA-SAST | IAM | Cloud / HSM-KMS | OT / ICS | Vendor / GRC | Discovery Notes |
|---|---|---|---|---|---|---|---|---|
| Network-facing TLS services | Primary | Partial | Limited | Limited | Partial | Partial | Limited | Tenable is the strongest scalable source for live, network-visible TLS services within authorized scan scope. |
| Leaf certificates presented on live services | Strong | Primary | Limited | Partial | Partial | Partial | Limited | Use Tenable to observe what is live; use PKI/CA records to validate what is issued and governed. |
| Root / intermediate CA posture | None | Primary | None | Partial | Partial | Limited | Limited | CA hierarchy, signing algorithms, trust chains, and issuance posture require authoritative PKI/CA data. |
| Certificate lifecycle policies and templates | None | Primary | None | Partial | Partial | Limited | Partial | Review certificate templates, issuance rules, enrollment, renewal, rotation, and revocation processes. |
| Embedded cryptographic libraries | Limited | None | Primary | None | Limited | Partial | Partial | SCA/SAST and code/dependency analysis provide the best visibility into embedded or statically linked libraries. |
| Hardcoded application cryptography | None | None | Primary | None | None | Limited | Partial | Requires application code review, SAST, architecture analysis, and application-owner validation. |
| Identity / federation signing and authentication crypto | Limited | Partial | Partial | Primary | Partial | None | Strong | IAM is authoritative for federation, token signing, certificate authentication, and identity trust configuration; vendor evidence supplements it. |
| HSM / KMS key inventory and algorithm configuration | None | Partial | Limited | Limited | Primary | Partial | Strong | HSM/KMS inventory is authoritative; vendor documentation helps validate supported algorithms and roadmap. |
| Cloud-managed certificates and service cryptography | Partial | Partial | Limited | Partial | Primary | None | Strong | Cloud-native inventory is strongest for managed keys, certificates, and service configuration; vendor evidence validates roadmap and service support. |
| OT / ICS protocol and device cryptography | Limited | Limited | Limited | None | Limited | Primary | Strong | OT asset, architecture, and engineering sources are primary; vendor documentation is critical for embedded firmware and long-lifecycle devices. |
| Vendor PQC roadmaps and attestations | None | None | None | Partial | Partial | Partial | Primary | Vendor/GRC is authoritative for collected roadmaps, attestations, support commitments, and contractual evidence. |

### Step 2 — Define Source Technology Extract Requirements
For each in-scope discovery source, define the exact extract
format, required fields, refresh cadence, and owner responsible
for producing the current file. The agent will consume only
these defined source technology extracts.

Refresh cadence should be appropriate to the source’s rate of
change, risk, and operational process. Event-driven refresh is
preferred for material changes.

Mark unavailable sources as coverage gaps. Do not invent data.

### Step 3 — PQC-CBOM Asset Classes
Classify every discovered item into one of the following
asset classes so the agent applies consistent logic:

| Asset Class | Example Assets |
|---|---|
| Certificate | TLS Certificate, SAML Signing Certificate, Code-Signing Certificate, OPC-UA Certificate, Smart Card Authentication Certificate |
| Key | RSA Key Pair, ECDSA Key Pair, AWS KMS Customer Managed Key (CMK), Azure Key Vault Key, HSM Code-Signing Key, Key-Wrapping Key |
| Cryptographic Library | OpenSSL, Bouncy Castle, wolfSSL, Botan, Libgcrypt, Java Cryptography Extension (JCE) |
| Cryptographic Algorithm | RSA, ECDSA, ECDH, Diffie-Hellman, AES-256, SHA-256, ML-KEM, ML-DSA |
| Certificate Authority | Enterprise Root CA, Enterprise Issuing CA, Intermediate CA, Public CA, Code-Signing CA |
| Trust Relationship | TLS Trust Chain, SAML Federation Trust, PKI Trust Anchor, Cross-Certification Trust, Certificate Chain of Trust |
| Cryptographic Protocol | TLS 1.2, TLS 1.3, IPsec, SSH, SAML, OIDC, OPC-UA Secure Channel |
| Cryptographic Service | AWS KMS, Azure Key Vault, Google Cloud KMS, Microsoft ADCS, Thales HSM, Entra ID Federation Service |
| Policy / Template | Web Server Certificate Template, Client Authentication Template, Code-Signing Policy, Key Rotation Policy, Certificate Renewal Policy |
| Vendor Attestation | PQC Support Roadmap, ML-KEM Support Statement, ML-DSA Support Statement, FIPS 140-3 Validation Evidence, Product Cryptographic Support Matrix |

### Step 4 — Agent Ingestion and Normalization
The Microsoft 365 Copilot agent (or supporting preprocessing)
ingests the current source technology extracts, normalizes
naming and identifiers, preserves evidence references and
collection dates, and prepares records for reconciliation.
Unavailable values remain `Unknown`.

The agent works with files uploaded in-session or made
available through approved enterprise integrations. It does
not maintain a persistent inventory database across sessions.

Generate Record ID from stable source identifiers such as
system ID, asset ID, certificate thumbprint, key ID,
application/component/version, or another documented composite
key. Do not use row position or ingestion order.

### Step 5 — Classify Cryptographic Function and Quantum-Vulnerable Status
For each discovered implementation the agent assigns:
- Asset class
- Cryptographic function(s)
- Current algorithm(s) and protocol/interface
- Quantum-vulnerable classification (Yes / No / Partial / Unknown / Not Applicable)
- PQC support evaluated by function where evidence permits:
  - PQC key-establishment support
  - PQC digital-signature support
  - PQC certificate/PKI support
  - Hybrid mode support
- If a single PQC Support field is used, set value to
  `Partially Supported` when support applies to only one
  function. Treat preview capability as `Preview`, not
  production-supported. Do not treat a vendor roadmap as
  deployed support.

Keep these as separate concepts. Do not collapse them:
- Quantum vulnerability
- PQC support
- Migration applicability
- Vendor roadmap status

### Step 6 — Multi-Source Reconciliation and Drift Detection
The agent performs reconciliation across sources using the
Discovery Coverage Matrix guidance and the following
precedence rule:

Use live observation to establish active deployment,
authoritative platform sources to establish governed
configuration, and vendor evidence to establish product
capability. Do not allow one source type to overwrite another
when they answer different questions.

Examples:
- Live leaf certificates ↔ CA-issued records
- SCA libraries ↔ deployed application versions
- KMS keys ↔ consuming applications
- Vendor claims ↔ deployed product/firmware versions
- OT assets ↔ protocol and certificate observations

**Allowed Reconciliation Status values only:**
- Validated
- Single-Source
- Shadow
- Stale
- Conflicting
- Unresolved

Do not create combined or free-form values such as Partially
Validated, Validated Constraint, Stale Candidate, or
Unresolved/Stale Candidate. Record constraints and migration
blockers in separate fields.

Suspected staleness without authoritative evidence is
classified as Reconciliation Status = Unresolved. Record
stale-candidate context separately as an assessment note.
Do not use Stale unless an authoritative source confirms
the record is no longer current.

When prior source snapshots are supplied, the agent produces
a delta view. Change types:

- **New** — record appears for the first time
- **Configuration Changed** — material algorithm, protocol,
  certificate, key, product version, owner, configuration,
  or lifecycle field changed
- **Observation Refreshed** — only scan metadata changed
  (scan ID, last-seen date, ingestion date, or evidence
  reference). Do not treat these as substantive configuration
  changes.
- **Drifted** — material change in risk or ownership context
- **Removed** — authoritative source confirms removal or
  decommissioning
- **No Longer Observed** — record was present previously but
  is absent from current extracts; do not assume decommission
  until confirmed by an authoritative source

### Step 7 — Ownership, Migration Dependency, and Priority
The agent maps technical and business owners (from CMDB or
supplied ownership data), records the primary migration
dependency, applies the evidence-based prioritization order,
and surfaces disposition recommendations.

Every actionable record requires a named technical owner.
If no owner is available from an authoritative source, record
Ownership Gap and create an assignment action.

Do not classify an evidence gap or reconciliation gap as an
Ownership Gap when a technical owner is already known.

Prioritization order:
1. Long-lived sensitive data and HNDL exposure
2. High-blast-radius trust infrastructure
3. Internet-facing or externally connected services
4. Long hardware/firmware/procurement lifecycle systems
5. Vendor-controlled or unsupported dependencies
6. Migration prerequisites that block multiple downstream systems

### Step 8 — Produce On-Demand CBOM Views and Answers
The agent returns:
- Source Extract Manifest
- Current CBOM view (component-level)
- Delta view (when prior snapshots exist)
- Discovery coverage and gap summary
- Migration dependency and owner action views
- Natural-language answers to operator and leadership queries

Every source referenced in the compiled view, findings,
ownership mapping, prioritization, or reconciliation must
appear in the Source Extract Manifest with a freshness state.
Source totals must reconcile to the final Manifest.

Do not state that all expected sources are available when any
expected source is Missing, Date Unknown, Stale, Aging, or
not represented in the Manifest. If a source classified as
Aging, Stale, Date Unknown, or Source Missing materially
affects confidence, disclose it in the Executive Summary.

### Continuous Operating Model
A living capability exists when:
- Source technology extracts have defined owners and refresh
  cadence appropriate to the source’s rate of change, risk,
  and operational process (event-driven refresh preferred for
  material changes)
- The agent can re-ingest and recompile on demand when current
  (and prior) extracts are provided
- Process ownership for keeping source extracts current is
  explicitly assigned
- Integration points exist with change management, certificate
  lifecycle, vulnerability management, and asset inventory
  processes so that source refreshes are reliable

No separate structured CBOM database is maintained. The
compiled view is ephemeral and recomputed from current
source technology extracts.

### Compiled Record Data Model

| Field | Required | Description / Permitted Values |
|---|---|---|
| Record ID | Yes | Deterministic identifier derived from stable source keys |
| System | Yes | Business service, application, or operational capability |
| Technology | Yes | Product, platform, library, protocol, or service |
| Asset Class | Yes | One of the defined PQC-CBOM Asset Classes |
| Component / Version | Recommended | Specific component, package, firmware, or version |
| Crypto Function | Yes | Key establishment, digital signature, authentication, etc. |
| Current Algorithm | Yes | Observed algorithm or Unknown |
| Protocol | Recommended | TLS, SSH, IPsec, SAML, OIDC, OPC-UA, etc. |
| Quantum-Vulnerable? | Yes | Yes / No / Partial / Unknown / Not Applicable |
| PQC Support | Yes | Supported / Partially Supported / Preview / Planned / Unsupported / Unknown / Not Applicable. When Partially Supported, note which function(s). |
| Discovery Source(s) | Yes | Source(s) that identified the implementation |
| Evidence Reference | Yes | Source record, file, query, report, or artifact reference |
| Source Extract Date | Yes | Date the source extract was produced |
| Agent Compilation Date | Yes | Date the agent compiled this view |
| Owner | Yes | Named technical owner or Ownership Gap |
| Migration Dependency | Yes | Specific dependency or Unknown |
| Disposition | Recommended | Retain and migrate / Upgrade / Refactor / Replace / Isolate / Compensate / Decommission / Investigate |
| Priority | Recommended | Critical / High / Medium / Low / Unscored |
| Reconciliation Status | Yes | Validated / Single-Source / Shadow / Stale / Conflicting / Unresolved |
| Confidence | Recommended | Confirmed / Corroborated / Single-Source / Inferred / Unknown |

### Minimum Viable Compilation Criteria
A record should not be treated as usable unless it contains
every Required field from the Compiled Record Data Model.

### Pre-Output Validation Gate
Before producing the final response, verify all of the following.
If any rule fails, correct the output before responding.
Validation failures must not be silently tolerated.

- Every material conclusion has an Evidence Reference
  traceable to a supplied source technology extract or
  authoritative supporting artifact.
- Every actionable record has a named technical owner or
  Ownership Gap.
- An evidence gap or reconciliation gap is not classified as
  an Ownership Gap when a technical owner is already known.
- Every Reconciliation Status uses an approved value only.
- Suspected staleness without authoritative evidence is
  Unresolved, not Stale.
- PQC Support is not generalized beyond the supported function(s).
- Quantum vulnerability, PQC support, migration applicability,
  and vendor roadmap status remain separate concepts.
- Vendor roadmap evidence is not represented as deployed capability.
- The compiled Component-Level PQC-CBOM includes every Required
  field from the Compiled Record Data Model.
- Every source used by the agent appears in the Source Extract
  Manifest with a freshness state.
- Source totals reconcile to the final Source Extract Manifest.
- Do not state that all expected sources are available when any
  expected source is Missing, Date Unknown, Stale, Aging, or
  absent from the Manifest.
- Aging, Stale, Date Unknown, or Source Missing sources that
  materially affect confidence are disclosed in the Executive
  Summary.
- Delta comparisons use only sources that have comparable prior
  snapshots.
- Observation Refreshed records are not classified as
  Configuration Changed.
- Removed records have authoritative removal evidence;
  otherwise use No Longer Observed.
- Every record included in a summary count exists in the
  applicable detailed output table.
- Executive Summary counts are derived directly from the final
  Delta View and Compiled CBOM View tables.
- No values or conclusions were invented.

## Output Format

Write in crisp direct declarative sentences. No marketing
language. No filler, minimal narrative. State findings and
actions — nothing else.

### Section 1 — Executive Summary
Three sentences maximum. State sources used, material coverage
gaps, highest-priority quantum-vulnerable dependencies or
drift items, and major migration blockers.
All counts must be derived directly from the final output tables.
Disclose material source-freshness limitations.

### Section 2 — Source Extract Manifest
| Source | File / Connection | Extract Date | Ingested Date | Scope | Owner | Freshness Status |
|---|---|---|---|---|---|---|

Freshness Status values: Current / Aging / Stale / Date Unknown / Source Missing

### Section 3 — Compiled PQC-CBOM View
| Record ID | System | Technology | Asset Class | Component / Version | Crypto Function | Current Algorithm | Protocol | Quantum-Vulnerable? | PQC Support | Discovery Source(s) | Evidence Reference | Source Extract Date | Agent Compilation Date | Owner | Migration Dependency | Disposition | Priority | Reconciliation Status | Confidence |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

### Section 4 — Delta View (when prior source snapshots exist)
| Record ID | Change Type | Prior State | Current State | Risk Implication | Required Action | Owner |
|---|---|---|---|---|---|---|

Change Type values: New / Configuration Changed / Observation Refreshed / Drifted / Removed / No Longer Observed

Delta summary counts in the Executive Summary must be calculated
exclusively from this table.

### Section 5 — Discovery Coverage Matrix (Actual)
| Technology Area | Primary Source Used | Supporting Sources | Coverage Rating | Known Gap | Follow-Up |
|---|---|---|---|---|---|

### Section 6 — Reconciliation Findings
| Record / Asset | Source A | Source B | Result | Risk | Required Action | Owner |
|---|---|---|---|---|---|---|

### Section 7 — Migration Dependency & Owner Action View
| Technology | Dependency | Constraint | Prerequisite | Owner | Vendor | Target State | Status |
|---|---|---|---|---|---|---|---|

## Quality Benchmark
A high-quality output recompiles cleanly from current source
technology extracts, explicitly surfaces coverage gaps and
reconciliation outcomes, names specific migration blockers
with owners, and produces actions specific enough to assign
to an engineer or team. Crisp direct declarative sentences.
No marketing language. No filler, minimal narrative.

Poor output:
"Several quantum-vulnerable algorithms were identified.
PQC migration planning should begin."

Good output:
"Agent run 14 Oct 2026 using current Tenable, CA export, and
SCA extracts. Source Extract Manifest shows CA export dated
12 Oct 2026 (Current) and SCA extract dated 3 Sep 2026 (Aging).
19 leaf certificates observed live remain absent from CA
inventory (Shadow). Configuration Changed: OpenSSL 1.1.1
detected in payment API (previously 3.0.x); quantum-vulnerable
RSA key exchange still present. OT gateway firmware remains on
ECDSA P-256 with vendor PQC support only in v4.x (EOS Q2 2028).
Recommend immediate CA reconciliation for the 19 Shadow
certificates, library upgrade owned by Payments Engineering,
and OT vendor roadmap validation owned by OT Security before
capital planning cycle."

## Known Limitations
- Quality is bounded by the currency and completeness of the
  source technology extracts. Stale or missing source files
  produce incomplete or outdated compiled views.
- The Microsoft 365 Copilot agent works with files provided
  in-session or through approved enterprise integrations; it
  does not maintain a persistent inventory database across
  sessions.
- Complex multi-source matching (especially across inconsistent
  naming) requires well-defined extract formats and may need
  human review for edge cases.
- OT/ICS discovery remains constrained by safety and availability
  requirements.
- Confidentiality shelf-life and HNDL prioritization require
  client-supplied data classification inputs where available.
- The agent does not replace the need for named process owners
  who keep the underlying source technology extracts current.
- This skill processes client data including potentially
  sensitive inventory information and scan output — handle all
  inputs in accordance with client data handling requirements
  and do not retain beyond the authorized process.

## Related Skills
- ai-bom-inventory.md (structural pattern and inventory
  discipline reference)
- nhi-risk-scorer.md (identity risk assessment for
  cryptographic signing and authentication components)

## Version History

| Version | Date | Change |
|---|---|---|
| 1.0 | August 2026 | Initial PQC-CBOM inventory skill |
| 1.1 | August 2026 | Technology-area Discovery Coverage Matrix; aligned structure to nhi-risk-scorer pattern |
| 1.2 | August 2026 | Shifted toward living system of record with versioned CBOM, drift detection, and continuous operating model |
| 1.3 | August 2026 | Architectural pivot: source technology extracts become the system of record; Microsoft 365 Copilot agent acts as on-demand compilation and query engine |
| 1.4 | August 2026 | Added explicit PQC-CBOM Asset Classes taxonomy; finalized Microsoft 365 Copilot agent capabilities and limitations language |
| 1.5 | August 2026 | Material reliability improvements: Source Extract Manifest, deterministic Record ID, source precedence, minimum viable criteria, Ownership Gap handling, No Longer Observed language, date separation, freshness states |
| 1.6 | August 2026 | Output Integrity & Determinism Controls: deterministic delta counts, Configuration Changed vs Observation Refreshed, strict statuses, required Evidence Reference, function-aware PQC Support, formal data model, Pre-Output Validation gate |
| 1.7 | August 2026 | Output-contract enforcement patch. No new analysis logic. Manifest completeness, required-field enforcement, concept separation, Ownership Gap vs evidence-gap distinction, suspected-staleness = Unresolved, no silent validation failures, Executive Summary freshness disclosure |
