---
name: pqc-cbom-inventory
description: Build and maintain a living Post-Quantum Cryptography
  Cryptographic Bill of Materials (PQC-CBOM) across IT,
  cloud, applications, identity, network, PKI, OT/ICS, and
  third-party services. Identifies quantum-vulnerable
  cryptographic dependencies, discovery evidence, ownership,
  PQC support status, and migration dependencies. Use when
  establishing or operating a continuous cryptographic
  inventory for PQC migration planning, reconciling multi-
  source discovery evidence, detecting drift, assessing
  quantum risk to long-lived data, mapping migration blockers,
  or producing evidence-based readiness posture against NIST
  PQC standards and CNSA 2.0. Accepts structured inventories
  (CSV, JSON, XLSX) and unstructured technical documentation,
  scan output, architecture records, vendor attestations, and
  interview notes. Treats the CBOM as a versioned, delta-driven
  system of record — not a point-in-time report.
license: MIT
compatibility: Designed for Claude Code, OpenAI Codex,
  and any Agent Skills compatible client. No system
  packages required. No network access required.
  Operates on provided inventory inputs and signal data only.
metadata:
  author: mike-poulos
  organization: Windval Technology Solutions
  version: "1.2"
  domain: cybersecurity
  subdomain: post-quantum-cryptography
  validated: "false"
  validation-score: pending
  compliance: NIST PQC, CNSA 2.0, NIST SP 800-208, NIST IR 8413
---
# Skill: PQC Cryptographic Bill of Materials (PQC-CBOM)

## Version
1.2 — August 2026

## Author
Mike Poulos, Executive Advisor — Cybersecurity
Windval Technology Solutions

## Purpose
Create and operate a living, evidence-based Cryptographic
Bill of Materials focused on Post-Quantum Cryptography
readiness. The PQC-CBOM is a versioned system of record —
not a point-in-time assessment report.

It inventories where cryptography is used, identifies
quantum-vulnerable public-key dependencies, records
ownership and discovery evidence, tracks migration
dependencies, and detects drift over time.

Readiness is a derived outcome of inventory quality and
maintenance discipline. Workflow order remains fixed:

> Inventory first. Validate second. Prioritize third. Migrate fourth.
> Then continuously reconcile and report deltas.

## Background
A single discovery platform or one-time assessment cannot
establish enterprise-wide PQC readiness. Network scanning,
CA exports, SCA results, HSM inventories, and vendor
roadmaps each see only part of the surface. Point-in-time
reports go stale the moment a certificate rotates, a
library is upgraded, firmware is patched, or a vendor
updates its PQC roadmap.

Living SBOMs solved the same problem for software
dependencies by shifting from periodic reports to
continuous, versioned, delta-driven inventories integrated
into change processes. The PQC-CBOM applies the same model
to cryptographic dependencies.

The CBOM closes the gap by maintaining, at component level:
- Technology or service that uses cryptography
- Cryptographic function performed
- Algorithms, protocols, certificates, keys, and libraries
- Quantum-vulnerable classification
- Discovery and validation source + evidence date
- Technical and business ownership
- PQC capability or vendor roadmap status
- Specific migration dependency
- Record version and last-reconciled date
- Drift status relative to prior version

## Parameters

- **INPUT:** One or more of the following — network cryptographic
  scan output, certificate inventory or CA export, application
  dependency inventory, HSM/KMS inventory, cloud cryptographic
  service export, IAM/federation configuration inventory,
  OT/ICS asset or architecture inventory, vendor product
  inventory or roadmap evidence, CMDB or asset inventory,
  prior CBOM version for delta analysis. Partial input is
  the normal starting condition — gaps in input are findings.
- **ENVIRONMENT:** enterprise / cloud / network / application /
  identity / PKI / OT/ICS / hybrid / all
- **SCOPE:** Technology domain, business unit, environment,
  application portfolio, or asset group
- **DISCOVERY_DEPTH:** baseline / validated / reconciled
- **MODE:** initial | delta | continuous
- **OUTPUT_FORMAT:** markdown / CSV / JSON / XLSX-ready table
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
Primary and Strong sources for each technology area.
Establish record granularity — a single product may generate
multiple CBOM records when it performs multiple cryptographic
functions.

Rule: Do not treat a vendor or product name as sufficient
evidence of cryptographic configuration.

| Technology / Discovery Area | Tenable | PKI / CA | AppSec / SCA-SAST | IAM | Cloud / HSM-KMS | OT / ICS | Vendor / GRC | Discovery Notes |
|---|---|---|---|---|---|---|---|---|
| Network-facing TLS services | Primary | Partial | Limited | Limited | Partial | Partial | Limited | Tenable is the strongest scalable source for live, network-visible TLS services within authorized scan scope. |
| Leaf certificates presented on live services | Strong | Primary | Limited | Partial | Partial | Partial | Limited | Use Tenable to observe what is live; use PKI/CA records to validate what is issued and governed. |
| Root / intermediate CA posture | None | Primary | None | Partial | Partial | Limited | Limited | CA hierarchy, signing algorithms, trust chains, and issuance posture require authoritative PKI/CA data. |
| Certificate lifecycle policies and templates | None | Primary | None | Partial | Partial | Limited | Partial | Review certificate templates, issuance rules, enrollment, renewal, rotation, and revocation processes. |
| Embedded cryptographic libraries | Limited | None | Primary | None | Limited | Partial | Partial | SCA/SAST and code/dependency analysis provide the best visibility into embedded or statically linked libraries. |
| Hardcoded application cryptography | None | None | Primary | None | None | Limited | Partial | Requires application code review, SAST, architecture analysis, and application-owner validation. |
| Identity / federation signing and authentication crypto | Limited | Partial | Partial | Primary | Partial | None | Strong | IAM is authoritative for federation, token signing, certificate authentication, and identity trust configuration; vendor evidence supplements it. |
| HSM / KMS key inventory and algorithm configuration | None | Partial | Limited | Limited | Primary | Partial | Strong | Native HSM/KMS inventory is authoritative; vendor documentation helps validate supported algorithms and roadmap. |
| Cloud-managed certificates and service cryptography | Partial | Partial | Limited | Partial | Primary | None | Strong | Cloud-native inventory is strongest for managed keys, certificates, and service configuration; vendor evidence validates roadmap and service support. |
| OT / ICS protocol and device cryptography | Limited | Limited | Limited | None | Limited | Primary | Strong | OT asset, architecture, and engineering sources are primary; vendor documentation is critical for embedded firmware and long-lifecycle devices. |
| Vendor PQC roadmaps and attestations | None | None | None | Partial | Partial | Partial | Primary | Vendor/GRC is authoritative for collected roadmaps, attestations, support commitments, and contractual evidence. |

### Step 2 — Ingest and Normalize Discovery Evidence
For each input source:
- Record source type, collection date, scope, and owner
- Extract technology, version, protocol, certificate, key,
  algorithm, and cryptographic-function details
- Normalize aliases and naming conventions
- Preserve the original evidence reference
- Mark unavailable values as `Unknown`; do not infer
  unsupported technical details

When MODE = delta or continuous, also ingest the prior
CBOM version for comparison.

### Step 3 — Create or Update Component-Level PQC-CBOM Records
Create one record for each material cryptographic
implementation. When a prior version exists, update
existing records and flag new, changed, or removed
dependencies.

Examples of record subjects:
- Load balancer TLS listener
- Root or issuing CA
- Application-bundled OpenSSL library
- SAML token-signing certificate
- HSM key used for code signing
- IPsec/IKE profile on a VPN gateway
- OPC UA server certificate and security policy
- Firmware-signing process for an OT device family

Every record must carry: Record ID, last-reconciled date,
CBOM version, and drift status (New / Unchanged / Changed /
Removed / Unresolved).

### Step 4 — Classify Cryptographic Function
Assign one or more functions:
- Key establishment / key exchange
- Digital signature
- Authentication
- Certificate trust
- Code or firmware signing
- Encryption at rest
- Encryption in transit
- Key wrapping / key protection
- Hashing / integrity
- Random-number generation
- Other / Unknown

### Step 5 — Identify Quantum-Vulnerable Dependencies
Record the observed algorithm and classify whether the
public-key dependency is quantum-vulnerable.

Common migration targets:
- RSA key establishment or signatures
- ECDSA signatures
- ECDH key agreement
- Finite-field Diffie-Hellman
- Vendor-specific public-key implementations requiring validation

Do not automatically classify symmetric encryption or hashing
as requiring replacement. Inspect any associated public-key
exchange, wrapping, certificate, signing, or authentication
dependency.

### Step 6 — Map the PQC Target
| Current Function | PQC Target Category |
|---|---|
| Public-key key establishment | ML-KEM or approved hybrid key-establishment mechanism |
| Digital signatures | ML-DSA, SLH-DSA, or another approved signature standard |
| Certificate signing | PQC-capable certificate and PKI implementation |
| Code / firmware signing | PQC-capable signing workflow and validation chain |
| Symmetric encryption | Retain appropriate symmetric algorithm; validate key-establishment and key-wrapping dependencies |
| Hashing / integrity | Retain appropriate hash algorithm; validate signature and certificate dependencies |

Record `Unknown` when the vendor, protocol, or architecture
has not selected a target.

### Step 7 — Reconcile Discovery Sources and Detect Drift
Reconciliation is mandatory where multiple sources cover the
same cryptographic asset. Use the Discovery Coverage Matrix
to identify which sources should be Primary or Strong.

When a prior CBOM version is supplied, perform delta analysis:
- New quantum-vulnerable dependencies
- Algorithm or protocol changes
- Ownership changes
- Migration dependency changes
- Records that disappeared (possible decommission or loss of visibility)
- Records that became Shadow or Stale

Classify reconciliation outcomes:
- `Validated` — discovery and authoritative records agree
- `Shadow` — observed live but absent from the expected system of record
- `Stale` — present in the system of record but no longer observed
- `Conflicting` — sources disagree
- `Unresolved` — insufficient evidence
- `Drifted` — material change since prior CBOM version

### Step 8 — Identify Migration Dependencies
Assign the primary migration dependency:
- Configuration change
- Protocol uplift
- Software/library upgrade
- Application refactor
- Certificate reissuance
- PKI hierarchy redesign
- HSM/KMS upgrade
- Hardware or firmware replacement
- Vendor roadmap dependency
- Third-party contract dependency
- Compatibility/interoperability testing
- Decommission
- Unknown

Record the specific technical or ownership constraint.
Do not collapse dependencies into a vague maturity score.

### Step 9 — Assess Risk Context
Document available evidence for:
- Data sensitivity
- Confidentiality shelf life
- External exposure
- Trust-chain or blast-radius significance
- System criticality
- Replacement complexity
- Vendor or lifecycle constraint
- Safety and availability constraints for OT/ICS

Where evidence is incomplete, leave the field `Unknown`
and create a follow-up action.

### Step 10 — Determine Disposition and Priority
Recommended dispositions:
- Retain and migrate
- Upgrade
- Refactor
- Replace
- Isolate
- Apply compensating control
- Decommission
- Investigate

Prioritization order (evidence-based):
1. Long-lived sensitive data and HNDL exposure
2. High-blast-radius trust infrastructure (roots, intermediates,
   identity signing, code signing)
3. Internet-facing or externally connected services
4. Systems with long hardware, firmware, or procurement lifecycles
5. Vendor-controlled or unsupported dependencies
6. Migration prerequisites that block multiple downstream systems

### Step 11 — Produce Outputs and Update the Living Record
Generate the structured outputs defined in Output Format.
When MODE = delta or continuous, emphasize changes since
the prior version. Persist the new CBOM version with
updated last-reconciled dates and drift status.

### Continuous Operating Model (Required for Living CBOM)
A PQC-CBOM is living only when the following are defined
and owned:

- **Triggers** — certificate rotation, library/firmware upgrade,
  new application or service deployment, vendor roadmap update,
  architecture change, scheduled discovery cycle (minimum quarterly)
- **Owner of the living process** — named team responsible for
  ingestion, reconciliation, and version publication
- **Delta reporting** — every cycle produces a clear list of
  New / Changed / Removed / Drifted records
- **Integration points** — change management, CI/CD (for
  application crypto), PKI certificate lifecycle, vulnerability
  management, and asset inventory processes
- **Access model** — queryable by owners and leadership
  (Microsoft Copilot agent or equivalent) with audit logging

If these elements are absent, the CBOM remains a high-quality
point-in-time baseline, not a living system of record.

## Output Format

Write in crisp direct declarative sentences. No marketing
language. No filler, minimal narrative. State findings and
actions — nothing else.

### Section 1 — Executive Summary
Three sentences maximum. State CBOM version, scope covered,
material coverage gaps, highest-priority quantum-vulnerable
dependencies or drift items, and major migration blockers.

### Section 2 — PQC-CBOM Inventory (Current Version)
Component-level system of record ordered by priority:

| Record ID | System | Technology | Component / Version | Crypto Function | Current Algorithm | Protocol | Quantum-Vulnerable? | PQC Support | Discovery Source | Validation Source | Owner | Migration Dependency | Disposition | Priority | Status | Last Reconciled | Drift |

### Section 3 — Delta Report (when prior version exists)
| Record ID | Change Type | Prior State | Current State | Risk Implication | Required Action | Owner |

### Section 4 — Discovery Coverage Matrix
| Technology Area | Primary Source Used | Supporting Sources | Coverage Rating | Known Gap | Follow-Up |

### Section 5 — Reconciliation Findings
| Record / Asset | Source A | Source B | Result | Risk | Required Action | Owner |

### Section 6 — Migration Dependency Register
| Technology | Dependency | Constraint | Prerequisite | Owner | Vendor | Target State | Status |

### Section 7 — Owner Action Queue
Evidence gaps, drift items, and remediation actions by
accountable team. Every actionable record requires a named
technical owner.

## Quality Benchmark
A high-quality output maintains a versioned inventory,
explicitly surfaces drift, names specific migration blockers
with owners, and produces actions specific enough to assign
to an engineer or team. Crisp direct declarative sentences.
No marketing language. No filler, minimal narrative.

Poor output:
"Several quantum-vulnerable algorithms were identified.
PQC migration planning should begin."

Good output:
"CBOM v1.4 reconciled 14 Oct 2026. 19 leaf certificates
observed live remain absent from CA inventory (Shadow) —
unchanged since v1.3. New drift: OpenSSL 1.1.1 library
detected in payment API (Record PQC-0847) previously
recorded as 3.0.x; quantum-vulnerable RSA key exchange
still present. OT gateway firmware still on ECDSA P-256
with vendor PQC support only in v4.x (EOS Q2 2028).
Recommend immediate CA reconciliation for the 19 Shadow
certificates, library upgrade owned by Payments Engineering,
and OT vendor roadmap validation owned by OT Security
before capital planning cycle."

## Known Limitations
- Network scan output alone is never a complete PQC-CBOM
- Vendor roadmap claims are evidence inputs, not proof of
  deployed capability
- Symmetric algorithms and hashes are not automatically
  migration targets; associated public-key dependencies must
  be inspected
- OT/ICS discovery is constrained by safety and availability
  requirements — specialized methods and vendor evidence are
  required
- Locally embedded or hardcoded cryptography invisible to
  network and PKI sources requires application or firmware
  analysis
- Confidentiality shelf-life and HNDL exposure assessments
  require data classification inputs that are frequently
  incomplete
- An unknown algorithm, owner, or migration path is itself
  a finding requiring follow-up
- Without defined triggers, process owner, and delta reporting,
  the CBOM reverts to a point-in-time baseline
- This skill processes client data including potentially
  sensitive inventory information, scan output, and interview
  notes — handle all inputs in accordance with client data
  handling requirements and do not retain beyond the
  engagement or authorized continuous process

## Related Skills
- ai-bom-inventory.md (structural pattern and inventory
  discipline reference)
- nhi-risk-scorer.md (identity risk assessment for
  cryptographic signing and authentication components)

## Version History

| Version | Date | Change |
|---|---|---|
| 1.0 | August 2026 | Initial PQC-CBOM inventory skill |
| 1.1 | August 2026 | Replaced source-centric table with technology-area Discovery Coverage Matrix; aligned structure to nhi-risk-scorer pattern |
| 1.2 | August 2026 | Shifted from engagement-bounded to living system of record model (SBOM-style). Added MODE parameter, drift detection, delta reporting, Continuous Operating Model requirements, versioned records, and explicit triggers/ownership for ongoing maintenance |
