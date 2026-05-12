# AI Agent Security Skills

AI agent skill files for cybersecurity use cases built
and maintained by Mike Poulos, Executive Advisor,
Cybersecurity at Windval Technology Solutions.

## What Are Skill Files?

Skill files are reusable markdown documents that encode
practitioner judgment into structured AI agent processes.
Each skill file defines a repeatable analysis workflow
that an AI agent executes against specific inputs,
producing consistent, auditable, framework-mapped outputs.

These are not prompts. They are parameterizable procedures
that function like method calls: same process, different
inputs, different client-specific outputs.

## Format and Compatibility

Skill files in this repository follow the
[Agent Skills open standard](https://agentskills.io)
published by Anthropic in December 2025 and adopted
by 32+ agent tools including Claude Code, OpenAI Codex,
GitHub Copilot, Cursor, Gemini CLI, and JetBrains Junie.

Each skill file includes YAML frontmatter with name,
description, license, compatibility, and metadata fields
that enable automatic discovery and activation by any
Agent Skills compatible client. The body content defines
the full practitioner workflow the agent executes.

All skill files are model-agnostic. They run without
modification on supported LLMs: Claude, GPT, Gemini,
or local models. No single model dependency.
No vendor lock-in.

## Validation

Completed skill files are validated against synthetic
test inventories with deliberately planted governance
gaps before any client use. Ground truth checklists
confirm expected output scores. Test inventories are
stored in the test-data/ directory alongside each
skill file.

## Skill Files

| Skill | Domain | Version | Status |
|---|---|---|---|
| [nhi-risk-scorer.md](nhi-risk-scorer.md) | Non-Human Identity Risk Scoring | v1.4 | Complete |
| [nhi-drift-monitor.md](nhi-drift-monitor.md) | Non-Human Identity Drift Detection | v1.3 | Complete |
| [ai-bom-inventory.md](ai-bom-inventory.md) | AI Asset Inventory | v1.4 | Complete |
| [ai-surface-monitor.md](ai-surface-monitor.md) | AI Attack Surface Expansion | — | Planned |
| [soc-decision-audit.md](soc-decision-audit.md) | SOC Decision Transparency | — | Planned |
| [identity-access-monitor.md](identity-access-monitor.md) | Identity and Access Control Health | — | Planned |
| [attacker-opportunity-index.md](attacker-opportunity-index.md) | Attacker Opportunity Index | — | Planned |
| [patch-kev-monitor.md](patch-kev-monitor.md) | Patch Compliance vs KEV Catalog | — | Planned |
| [ics-threat-brief.md](ics-threat-brief.md) | OT/ICS Threat Intelligence | — | Planned |
| [nerc-cip-gap-analysis.md](nerc-cip-gap-analysis.md) | NERC CIP Compliance Gap Analysis | — | Planned |
| [vuln-discovery-source-review.md](vuln-discovery-source-review.md) | AI-Assisted Vulnerability Discovery | — | Planned |
| [vulnops-program-assessment.md](vulnops-program-assessment.md) | VulnOps Program Maturity | — | Planned |
| [agent-data-access-assessment.md](agent-data-access-assessment.md) | Agent Data Access Security Assessment | — | Planned |
| [agent-architecture-governance.md](agent-architecture-governance.md) | Agent Architecture Governance | — | Planned |
| [agent-human-override-design.md](agent-human-override-design.md) | Agent Human Override and Approval Design | — | Planned |

## Domain Coverage

- Non-Human Identity (NHI) governance, risk scoring,
  and drift detection
- AI Bill of Materials and agentic asset inventory
- AI attack surface monitoring
- SOC decision transparency and audit trail
- Identity and access control health monitoring
- Attacker opportunity index and prioritization
- Patch compliance against active exploitation catalog
- OT/ICS threat intelligence analysis and briefing
- NERC CIP compliance gap analysis
- AI-assisted vulnerability discovery and source code analysis
- VulnOps program maturity assessment
- Agent data access security assessment
- Agent architecture governance and design review
- Agent human override and approval workflow design

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

## References

Pipeline configurations, system prompts, and supporting
artifacts for skill files and related agentic workflows.

- [ics-threatintel-system-prompt.md](references/ics-threatintel-system-prompt.md) — OT/ICS threat intelligence system prompt extracted from production n8n pipeline. Includes executive summary structure, quality benchmark, findings table format, and pipeline configuration reference.
- [ics-threatintel-pipeline.json](references/ics-threatintel-pipeline.json) — n8n workflow export for OT/ICS threat intelligence briefing pipeline. Import directly into any n8n instance to restore the production workflow.

## Version History

| Date | Change |
|---|---|
| April 2026 | Initial release — nhi-risk-scorer.md v1.0, ai-bom-inventory.md v1.0 |
| April 2026 | nhi-risk-scorer.md v1.1 through v1.3 — agentic correlation, version correction, credential elevation rule |
| April 2026 | ai-bom-inventory.md v1.1 through v1.3 — OT adversarial signal, agent taxonomy, M365 Copilot agentic note |
| April 2026 | Added monitoring skill files — nhi-drift-monitor, ai-surface-monitor, identity-access-monitor, patch-kev-monitor, attacker-opportunity-index |
| April 2026 | Added vuln-discovery-source-review.md and vulnops-program-assessment.md |
| April 2026 | Reorganized skill file table by domain grouping, added Version column |
| April 2026 | All three completed skill files updated to agentskills.io YAML frontmatter compliance |
| May 2026 | Added planned skill files — agent-data-access-assessment, agent-architecture-governance, agent-human-override-design |
| May 2026 | Added references/ directory — ICS threat intel system prompt and n8n pipeline JSON extracted and preserved from production deployment |

## Related Work

- Human Identity is Governed. Machine Identity is Not. Attackers Already Know.
  NHI Security Perspective Paper (Windval, 2026)
- OT/ICS Threat Intelligence Pipeline — automated US-CERT/CISA feed
  analysis with NERC CIP and MITRE ATT&CK for ICS mapping

## License

MIT
