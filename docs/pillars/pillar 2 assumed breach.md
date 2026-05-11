# Pillar 2 — Assumed Breach

## Definition

Pillar 2 requires that systems be designed and operated as though the
adversary is already inside the perimeter. Controls must function under
the assumption of compromise, not merely the assumption of prevention.

## Federal Standards Lineage

- NIST SP 800-137 Rev 1 — continuous monitoring
- NIST SP 800-53 Rev 5 IR and SI control families
- CISA Joint Cyber Defense Collaborative (JCDC) AI exercises
- CISA August 2024 joint event-logging guide
- DoD PfMO Purple Team Reviews
- RAND SL1–SL5 weight-security benchmark assessments

## Existing Federal Control Architecture

NIST SP 800-53 Rev 5 IR (Incident Response) and SI (System and
Information Integrity) control families form the baseline. Continuous
monitoring under NIST SP 800-137 Rev 1 provides the detection layer.
CISA's joint event-logging guide establishes the telemetry requirements.
DoD PfMO Purple Team Reviews operationalize adversarial testing.
JCDC AI exercises extend these controls specifically to AI systems.
RAND SL1-SL5 weight-security benchmark assessments provide the
AI-specific security level taxonomy.

## What Is Genuinely New for AI

Weight as a single point of catastrophic loss (a one-shot capability
transfer with no clean federal-doctrine analogue); the probabilistic,
non-deterministic enforcement boundary; training data as a long-fuse
attack vector operating on release-cycle timescales beyond standard SOC
windows; agent excessive agency creating credential blast-radius
interactions that zero-trust per-session controls handle only partially.

## Evidence Artifacts Required

- Incident response plan with AI-specific playbooks
- Continuous monitoring strategy covering model behavior
- Event logging architecture meeting CISA joint logging guidance
- Purple team or red team assessment results
- AI Bill of Materials (AIBOM) for supply chain visibility
- Weight security assessment mapped to RAND SL1-SL5 levels
- Breach simulation tabletop exercise records

## Interaction with Other Pillars

Pillar 2 is the runtime complement to Pillar 1's design-time
commitments. Zero trust (Pillar 4) provides the per-session
enforcement that limits blast radius when breach occurs. Pillar 3
(no single point of failure) ensures that a compromised component
does not bring down the entire system. Pillar 5 draws on incident
response logs and breach drill artifacts as core accountability
evidence — the 72-hour DFARS reporting clock starts the moment
assumed-breach controls detect a real compromise.
