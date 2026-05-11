# Pillar 1 — Governance, Security, and Privacy Baked In

## Definition

Pillar 1 holds that governance, security, and privacy must be properties
of the system as engineered — not properties of the documents written
about it. Three sub-lineages converge here: secure-by-design,
privacy-by-design, and systems security engineering.

## Federal Standards Lineage

- CISA Secure-by-Design white paper (April 2023) and RSA Pledge (May 8, 2024)
- Cavoukian's seven privacy-by-design principles → GDPR Article 25 → NIST Privacy Framework 1.0 (January 2020) → PFW 1.1 IPD (April 2025)
- NIST SP 800-160 Vol 1 Rev 1 (November 2022) — systems security engineering
- NIST AI RMF 1.0 (January 2023) — GOVERN, MAP, MEASURE, MANAGE functions
- ISO/IEC 42001 (December 2023) — AI management system standard

## Existing Federal Control Architecture

The NIST AI RMF 1.0 GOVERN function covers organizational policies,
roles, and accountability structures. The MAP function requires
contextualizing AI risks. MEASURE establishes metrics for ongoing
monitoring. MANAGE defines response and recovery processes. ISO/IEC
42001 adds a certifiable management system layer. NIST SP 800-160
Vol 1 Rev 1 operationalizes security as a system property through
its stakeholder needs, system requirements, and architecture processes.

## What Is Genuinely New for AI

Traditional secure-by-design assumes a deterministic system where
behavior can be fully specified. AI systems are probabilistic —
behavior emerges from training data and cannot be fully specified
in advance. This means governance must be baked into the training
pipeline, data provenance chain, and model evaluation process, not
only the deployment architecture. Model cards, datasheets for
datasets, and AI impact assessments are the new artifact class
that secure-by-design generates for AI that has no direct analogue
in traditional systems engineering.

## Evidence Artifacts Required

- Model card for each deployed model
- Datasheet for each training dataset
- AI impact assessment completed pre-deployment
- Privacy impact assessment (PIA) with AI supplement
- System security plan (SSP) sections covering AI components
- NIST AI RMF profile mapping GOVERN/MAP/MEASURE/MANAGE controls

## Interaction with Other Pillars

Pillar 1 is the design-time foundation. Pillars 2 and 4 (assumed
breach and zero trust) are runtime enforcement mechanisms that only
function correctly if Pillar 1's architectural commitments are
present at design time. Pillar 3 (no single point of failure)
depends on Pillar 1 having specified diversity and redundancy as
system requirements rather than operational afterthoughts. Pillar 5
(360° accountability) draws its evidence chain from the artifacts
Pillar 1 generates — model cards, datasheets, and impact assessments
are the primary inputs to the accountability record.
