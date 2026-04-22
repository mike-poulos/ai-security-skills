# AI Agent Security Skills

AI agent skill files for cybersecurity advisory engagements, 
built and maintained by Mike Poulos, Executive Advisor,
Cybersecurity at Windval Technology Solutions.

## What Are Skill Files?
Skill files are reusable markdown documents that encode 
practitioner judgment into structured AI agent processes. 
Each skill file defines a repeatable analysis workflow 
that an AI agent executes against client-specific inputs, 
producing consistent, auditable, framework-mapped outputs.

These are not prompts. They are parameterizable procedures 
that function like method calls — same process, different 
inputs, different client-specific outputs.

## Skill Files
| Skill | Domain | Status |
|---|---|---|
| [nhi-risk-scorer.md](nhi-risk-scorer.md) | Non-Human Identity | Complete |
| [ai-bom-inventory.md](ai-bom-inventory.md) | AI BOM Asset Inventory | Complete |
| [soc-decision-audit.md](soc-decision-audit.md) | SOC / Decision Transparency | In Development |
| [ics-threat-brief.md](ics-threat-brief.md) | OT/ICS Threat Intelligence | Planned |
| [nerc-cip-gap-analysis.md](nerc-cip-gap-analysis.md) | NERC CIP Compliance | Planned |
| [vuln-discovery-source-review.md](vuln-discovery-source-review.md) | AI-Assisted Vulnerability Discovery | Planned |
| [vulnops-program-assessment.md](vulnops-program-assessment.md) | VulnOps Program Maturity | Planned |

## Domain Coverage
- Non-Human Identity (NHI) governance and risk scoring
- AI Bill of Materials and agentic asset inventory
- OT/ICS threat intelligence analysis and briefing
- NERC CIP compliance gap analysis
- SOC decision transparency and audit trail
- AI-assisted vulnerability discovery and source code analysis
- VulnOps program maturity assessment

## Practitioner Background
Security architecture and engineering, network security, 
zero trust architecture, IAM/PAM, directory services, 
SIEM/SOC, EDR/EPP, CNAPP/cloud security, FedRAMP, 
vulnerability management, application security, 
container security, NHI security, GRC, NERC CIP compliance, 
OT/ICS security, IEC 62443, NIST CSF, configuration 
auditing, device profiling.

## Design Principles
These skill files are model-agnostic by design.
They encode practitioner judgment as structured
process, not model-specific configuration. They
run on any capable LLM — Claude, GPT, Gemini,
or local models — without modification. No single
model dependency. No vendor lock-in.

## Related Work
- The Silent Majority: NHI Security white paper (Windval, 2026)
- OT/ICS Threat Intelligence Pipeline — automated CISA feed 
  analysis with NERC CIP and MITRE ATT&CK for ICS mapping

## License
MIT
