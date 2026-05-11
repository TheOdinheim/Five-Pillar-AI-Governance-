# Pillar 4 — Zero Trust Throughout

## Definition

Pillar 4 applies "never trust, always verify" to humans, services, and
AI agents equally. The extension to AI agents — which can acquire
credentials, make autonomous decisions, and act across system boundaries
— is where this pillar goes beyond existing zero-trust doctrine.

## Federal Standards Lineage

- NIST SP 800-207 (August 2020) — seven tenets and PE/PA/PEP model
- NIST SP 800-207A (September 2023) — identity-centric segmentation in cloud-native multi-cloud
- NIST SP 1800-35 (final June 2025) — 19 reference implementations from 24 industry collaborators
- OMB M-22-09 (January 26, 2022) — FY24 deadline extended via NSM-8 to DoD/IC
- CISA Zero Trust Maturity Model 2.0 (April 2023) — five pillars × four maturity stages × three cross-cutting capabilities
- DoD Zero Trust Strategy (November 2022) — FY27 Target / FY32 Advanced, 152 capabilities; DTM 25-003 (effective July 17, 2025)
- NSA Zero Trust Implementation Guideline Primer (January 8, 2026) — five execution phases

## Existing Federal Control Architecture

The seven 800-207 tenets: all data sources and computing services are
resources; all communication is secured regardless of network location;
per-session access; dynamic policy including observable client identity
and behavioral attributes; monitoring and measuring integrity of assets;
strict authentication and authorization before access is granted;
collection of information to improve security posture.

## What Is Genuinely New for AI

Agent excessive agency — AI agents that can acquire credentials and act
autonomously create credential blast-radius interactions that
per-session tenets address only partially. Embedding/semantic anomaly
detection is a new telemetry class with no direct equivalent in
traditional ZT implementations.

## Evidence Artifacts Required

- Zero trust architecture diagram mapping PE/PA/PEP components
- CISA Zero Trust Maturity Model 2.0 self-assessment with current
  and target maturity levels for each of the five CISA ZT pillars
- Identity governance documentation covering human and AI agent
  identities, including SPIFFE-based workload identity where applicable
- Per-session access policy definitions for AI agent interactions
- Behavioral analytics baseline for AI agent activity patterns
- Embedding/semantic anomaly detection architecture and thresholds
- Network segmentation documentation showing AI workload isolation
- Authentication and authorization policy for agent-to-agent and
  agent-to-service communication

## Interaction with Other Pillars

Pillar 4 is the enforcement mechanism for Pillars 1 through 3.
Governance baked in (Pillar 1) defines the policies that zero trust
enforces at runtime. Assumed breach (Pillar 2) provides the threat
model that justifies zero trust's "never trust" posture — without
the assumed breach mindset, zero trust controls appear excessive
and face organizational resistance. No single point of failure
(Pillar 3) ensures the zero trust infrastructure itself is not a
concentration risk — a single policy decision point (PDP) failure
must not disable all access control. Pillar 5 (360° accountability)
requires that zero trust logs, session records, and policy decisions
feed into the auditable evidence chain so that every access grant
or denial is traceable to a responsible party.
