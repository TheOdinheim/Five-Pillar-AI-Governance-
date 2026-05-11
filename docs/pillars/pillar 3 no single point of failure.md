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

NIST SP 800-160 Vol 2 Rev 1 names Diversity explicitly as one of 14
cyber-resiliency techniques alongside Redundancy, Segmentation,
Coordinated Protection, and Adaptive Response. NIST SP 800-34 Rev 1
operationalizes this through contingency planning constructs: business
impact analysis (BIA), recovery time objectives (RTO), recovery point
objectives (RPO), and maximum tolerable downtime (MTD). NIST SP
800-161 Rev 1 upd1 extends these concepts into supply-chain risk
management. EO 14028 established the SBOM lineage through NTIA July
2021 minimum elements. The CISA 2025 SBOM NPRM proposes codifying
minimum SBOM elements into regulation but remains a proposed rule
as of May 2026.

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

- AI Bill of Materials (AIBOM) for each deployed model
- Software Bill of Materials (SBOM) for each AI system component
- Information system contingency plan (ISCP) with AI-specific annexes
- Cybersecurity supply-chain risk management (C-SCRM) plan
- Separation of duties (SoD) matrix covering AI operations
- Concentration risk register identifying single-provider dependencies
- Tested failover documentation for multi-model or multi-region setups
- SLSA provenance attestations at L2 or higher where applicable

## Interaction with Other Pillars

Pillar 3 is in direct tension with operational simplicity. Pillar 1
(governance baked in) must specify diversity and redundancy as system
requirements at design time or Pillar 3 becomes an unfunded mandate
at deployment. Pillar 2 (assumed breach) depends on Pillar 3 to
ensure that a single compromised model, provider, or cloud region
does not cascade into total system failure. Pillar 4 (zero trust)
reinforces Pillar 3 by preventing a compromised component from
laterally accessing redundant backups. Pillar 5 (360° accountability)
must allocate responsibility for concentration risk across the value
chain — particularly when a prime contractor selects a single
foundation-model provider and flows that dependency down to every
subcontractor.
