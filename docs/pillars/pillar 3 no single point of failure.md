# Pillar 3 — No Single Point of Failure

## Definition

Pillar 3 demands resilience, redundancy, and the absence of
concentration risk in the deployment substrate. Diversity is treated as
a security technique, not merely an operational preference.

## Federal Standards Lineage

- NIST SP 800-160 Vol 2 Rev 1 (December 2021) — 14 cyber-resiliency techniques including Diversity, Redundancy, Segmentation, Coordinated Protection, and Adaptive Response
- NIST SP 800-34 Rev 1 (May 2010) — contingency planning (BIA, RTO, RPO, MTD)
- NIST SP 800-161 Rev 1 upd1 (November 2024) — cybersecurity supply-chain risk management
- EO 14028 (May 12, 2021) — SBOM lineage via NTIA July 2021 minimum elements
- CISA 2025 Minimum Elements for an SBOM NPRM (August 22, 2025)

## Existing Federal Control Architecture

*[To be completed.]*

## What Is Genuinely New for AI

Foundation-model concentration (~88% top-3 providers), compute
concentration (NVIDIA + TSMC CoWoS chokepoints), and inference-cloud
concentration substantially exceed what federal cybersecurity standards
were designed to handle. The defensible posture is layered: L1
table-stakes (AIBOM, SBOM, ISCP, C-SCRM plan, SoD matrix); L2
resilience-engineered (SLSA L2+ with Sigstore, multi-region inference,
multi-model abstraction with tested failover, concentration risk
register); L3 federated/distributed (multi-vendor diversity,
multi-cloud, PPFL where data sovereignty applies).

## Evidence Artifacts Required

*[To be completed.]*

## Interaction with Other Pillars

*[To be completed.]*
