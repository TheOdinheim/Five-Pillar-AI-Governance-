# Five-Pillar Federal AI Governance Framework

> Governance is a property of the deployment substrate, enforced by the
> architecture and witnessed by 360-degree evidence — never a property
> of the policy document.

## What This Is

A federally-defensible architectural framework for AI deployment,
grounded in primary federal sources (NIST, OMB, CISA, NSA, DoD CIO,
Treasury, GSA) and international standards (ISO/IEC 42001, EU AI Act,
GDPR). The five pillars define the smallest set of architectural
commitments that produce a defensible federal AI deployment as of
May 2026.

## The Five Pillars

| # | Pillar | Core Principle |
|---|--------|----------------|
| 1 | [Governance, Security & Privacy Baked In](docs/pillars/pillar-1-governance-security-privacy.md) | These are properties of the system as engineered, not of the documents written about it |
| 2 | [Assumed Breach](docs/pillars/pillar-2-assumed-breach.md) | Design and operate as though the adversary is already inside |
| 3 | [No Single Point of Failure](docs/pillars/pillar-3-no-single-point-of-failure.md) | Resilience, redundancy, and the absence of concentration risk in the deployment substrate |
| 4 | [Zero Trust Throughout](docs/pillars/pillar-4-zero-trust.md) | Never trust, always verify — applied to humans, services, and AI agents equally |
| 5 | [360° Accountability](docs/pillars/pillar-5-360-accountability.md) | Developer, deployer, contractor, vendor, regulator, and end-user all share auditable obligations through a single evidence chain |

## Empirical Anchor

The Mythos / Glasswing / Mercor incident chain (March–April 2026) is
the first public AI supply-chain compromise to fail across all five
pillars simultaneously. Its forensic reconstruction is referenced
throughout this framework as a concrete illustration of what
architectural gaps look like at scale.

## Who This Is For

Federal agency AI leads, contracting officers, system owners preparing
ATO packages, and contractors building procurement-ready AI governance
documentation.

## How to Navigate

- **[docs/pillars/](docs/pillars/)** — The substantive framework content, one file per pillar
- **[docs/artifacts/](docs/artifacts/)** — Evidence templates and compliance checklists
- **[docs/references/](docs/references/)** — Consolidated federal standards lineage
- **[CONTRIBUTING.md](CONTRIBUTING.md)** — How to contribute to this framework

## License

Apache 2.0 — see [LICENSE](LICENSE).
