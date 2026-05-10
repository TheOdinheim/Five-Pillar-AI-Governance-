# Contributing to the Five-Pillar Federal AI Governance Framework

Thank you for your interest in contributing. Because this framework is
used in federal procurement and ATO contexts, precision of citation and
structural consistency are the two most important contribution standards.

## Citation Standards

Every factual claim in the pillar documents must link to a primary
federal source — the original NIST SP, OMB memo, EO text, or CISA
publication. Do not link to secondary sources (blog posts, vendor
summaries, aggregator sites) as the primary citation. If a secondary
source is useful for context, it may appear as a supplemental link after
the primary citation.

When a standard has been updated or superseded, update the citation in
full. Do not leave outdated references alongside updated ones.

## Where Different Content Belongs

Pillar definition files in `docs/pillars/` contain: definitions,
federal standards lineage, existing control architecture, genuinely new
AI-specific requirements, and required evidence artifacts. They do not
contain implementation guidance — that belongs in `docs/artifacts/`.

`docs/artifacts/checklists/` contains practitioner-facing checklists
derived from each pillar. `docs/artifacts/evidence-templates/` contains
templates for the specific evidence artifacts each pillar requires.

## Pull Request Process

Tag which pillar your PR touches in the title, e.g.:
`[Pillar 3] Update SBOM NPRM citation to reflect final rule`

If your PR touches the cross-pillar interaction matrix or the synthesis
section, tag it `[Cross-Pillar]`.

## Reporting Citation Gaps or Outdated Standards

Use the "Citation Gap or Outdated Standard" issue template. Federal
standards move frequently — rescissions, supersessions, and final rules
that differ from drafts are the most common gap category.
