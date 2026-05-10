# Pillar 5 — 360 Accountability

## Definition

Pillar 5 holds that developer, deployer, contractor, vendor, regulator,
and end-user all share auditable obligations that flow through a single
evidence chain. As of May 2026, no single U.S. statute or regulation
allocates accountability across the full AI value chain — this pillar
identifies what a complete allocation would require and where the
current gaps are.

## Federal Standards Lineage

- EU AI Act (Regulation 2024/1689) — provider/deployer split; Article 53 GPAI obligations operative since August 2, 2025
- NIST AI RMF GOVERN function — GV.RR roles and responsibilities, GV.PO policies, GV.SC supply chain, GV.RM risk management
- GDPR controller/processor split (Articles 4(7), 4(8), 24, 28)
- GPAI Code of Practice (finalized July 10, 2025)
- FAR 52.244-6 — flow-down for commercial items
- DFARS 252.204-7012 / 7021 — culminating in CMMC 2.0 (32 CFR Part 170, final October 15, 2024, effective December 16, 2024)

## Existing Accountability Allocation Frameworks

The EU AI Act's Article 3 distinguishes provider (places system on
market) from deployer (uses under their authority). High-risk
obligations (Articles 8–21) fall on providers; deployer obligations
(Article 26) include using systems per provider instructions, assigning
human oversight, monitoring, and informing affected persons.

The federal contractor flow-down architecture is: FAR 52.204-21 basic
safeguarding; FAR 52.204-25 Section 889; DFARS 252.204-7012
(cyber-incident reporting, 72-hour mandatory subcontract flow-down);
DFARS 252.204-7021 (effective November 10, 2025, phased to November 10,
2028 — prime must verify subcontractor CMMC status before subaward).

## What Is Genuinely New for AI

No existing U.S. framework allocates accountability to compliance
certifiers, contractor data labelers, or open-source gateways — three
of the five Mercor defendants. The downstream provider-becomes-provider
rule in the EU AI Act (substantial fine-tune = more than one-third of
original training compute) has no U.S. equivalent.

## Evidence Artifacts Required

*[To be completed.]*

## Interaction with Other Pillars

Pillar 5 is the evidence spine for the other four. Each of Pillars 1–4
generates artifacts (system design documents, incident logs, ZT
architecture diagrams, BOM/SBOM/AIBOM records) that Pillar 5's
accountability chain requires as inputs. Without the other four
functioning correctly, Pillar 5 accountability becomes a paper exercise.

## Open Design Questions

- How should multi-agent orchestration chains be represented in an
  accountability allocation matrix?
- What is the appropriate accountability tier for open-source model
  distributors who provide weights but not deployment infrastructure?
- How should accountability shift when a deployer fine-tunes beyond the
  one-third-of-compute threshold?
