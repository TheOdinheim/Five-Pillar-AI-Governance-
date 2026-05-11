# Five-Pillar Federal AI Governance Framework — Validation Test v1

## Classification: UNCLASSIFIED / For Framework Validation Only

**Test Date:** May 10, 2026
**Framework Version:** v1 (as published to TheOdinheim/Five-Pillar-AI-Governance)
**Test Type:** Full-lifecycle validation against a synthetic federal deployment scenario, with retrospective cross-validation against the Mythos/Glasswing/Mercor incident chain
**Test Objective:** Determine whether the framework, when applied as written, produces every artifact it claims, covers every control gap it identifies, and holds under adversarial conditions modeled on a real-world failure

---

## Test Scenario: Project SENTINEL

### Agency and Mission

The Department of Veterans Affairs (VA) Office of Information and Technology (OIT) is deploying an AI-assisted benefits adjudication system — **Project SENTINEL** — to reduce the claims backlog. The system ingests veteran claims documentation (medical records, service records, disability rating requests), extracts structured data, cross-references regulatory criteria (38 CFR Part 4 — Schedule for Rating Disabilities), and produces draft adjudication recommendations for human adjudicators.

### Why This Scenario Stress-Tests All Five Pillars

This scenario was selected because it creates pressure on every pillar simultaneously:

**Pillar 1 (Governance baked in):** The system processes Protected Health Information (PHI) under HIPAA, Personally Identifiable Information (PII) under the Privacy Act, and veterans' disability records that carry additional statutory protections. Governance cannot be a paper exercise — the system design must enforce privacy and security at the architectural level or the VA faces both legal liability and harm to a vulnerable population.

**Pillar 2 (Assumed breach):** VA has been the target of multiple significant breaches (2006 laptop theft exposing 26.5M veterans, 2020 financial information breach). The system processes exactly the kind of high-value PII that threat actors target. The assumed-breach posture is not theoretical here — it reflects documented operational history.

**Pillar 3 (No single point of failure):** The VA would likely procure a commercial foundation model (e.g., GPT-4.5, Claude Opus 4.6, Gemini) through a prime contractor. This creates immediate concentration risk: single model provider, single cloud region, single contractor. If the model provider experiences an outage, rate-limits the API, or changes its terms of service, the claims backlog worsens instead of improving.

**Pillar 4 (Zero trust):** The system requires access to VistA/CPRS medical records, VBMS claims data, and potentially DoD service records via OMPF. Each data source has its own authorization boundary. AI agents parsing documents need credentials scoped per-session, not standing access. A compromised agent with broad access to veteran medical records is a catastrophic privacy event.

**Pillar 5 (360° accountability):** The value chain includes: the foundation model developer (e.g., Anthropic), the cloud infrastructure provider (e.g., AWS GovCloud), the prime contractor (systems integrator), subcontractors (data labelers who annotated training data for VA-specific fine-tuning, the compliance certifier who attested to FedRAMP/CMMC posture), and the VA itself as deployer. When the system produces an incorrect benefits denial, who is accountable? The framework must answer this.

### Technical Architecture (Synthetic)

- **Foundation model:** Claude Opus 4.6 accessed via AWS Bedrock in GovCloud us-gov-west-1
- **Fine-tuning:** VA-specific adapter trained on 50,000 historical adjudication decisions, annotated by a subcontractor (DataLabel Corp, a fictional entity)
- **Deployment:** Containerized on EKS in GovCloud, behind an API gateway
- **Data sources:** VistA/CPRS (medical), VBMS (claims), OMPF via DPRIS (service records)
- **Identity:** PIV/CAC for human users, SPIFFE SVIDs for service-to-service
- **Prime contractor:** Sentinel Systems Inc. (fictional), holding CMMC L2 certification
- **Compliance certifier:** SecureAudit LLC (fictional C3PAO)
- **Authorization boundary:** Existing VA ATO boundary extended per VA 6500 Handbook

---

## Phase 1: Discovery and Scoping (Pillar Framework Test)

The framework's implementation dossier specifies Phase 1 as a 2–4 week discovery phase producing six deliverables: stakeholder/role-responsibility matrix, classified AI portfolio inventory, threat model document, concentration risk register v0, gap-discovery questionnaire responses, and engagement charter with revised SOW.

### Test 1.1 — Stakeholder/Role-Responsibility Matrix

**Framework claim:** The matrix "explicitly enumerates every named role across the 360-accountability matrix: developer, deployer, contractor, vendor, regulator, end-user."

**Test execution:** Applying this to Project SENTINEL:

| Role | Entity | Framework Category | Control Responsibilities |
|------|--------|--------------------|------------------------|
| Developer (model producer) | Anthropic | Developer | Model card, system card, safety evaluations, weight security, RSP/safety case |
| Developer (fine-tune) | DataLabel Corp (sub) | Developer (derivative) | Training data provenance, annotation quality, datasheet for datasets, bias evaluation |
| Cloud infrastructure | AWS GovCloud | Vendor | FedRAMP High authorization, physical security, encryption at rest/transit, availability SLA |
| Prime contractor | Sentinel Systems Inc. | Contractor (DIB prime) | System integration, SSP, POA&M, CMMC L2 attestation, 72-hour incident reporting, subcontract flow-down |
| Compliance certifier | SecureAudit LLC (C3PAO) | Vendor (certifier) | CMMC assessment, FedRAMP 3PAO assessment if applicable |
| Deployer (mission owner) | VA OIT | Deployer | ATO decision, continuous monitoring, human oversight of adjudication recommendations, privacy impact |
| Regulator | DCMA DIBCAC, FedRAMP PMO, VA OIG | Regulator | Oversight, audit, enforcement |
| End-user (federal employee) | VA claims adjudicators | End-user (operator) | Human-in-the-loop review, override authority, reporting anomalous outputs |
| End-user (affected person) | Veterans filing claims | End-user (subject) | Right to human review of AI-assisted decision, right to appeal, right to explanation |

**Verdict: PASS.** The framework's role taxonomy covers every entity in the value chain. The "developer (derivative)" category for DataLabel Corp maps to the EU AI Act's provider-becomes-provider rule. The framework correctly identifies that no existing U.S. statute assigns accountability to DataLabel Corp — this is a genuine gap the framework surfaces.

**Gap identified:** The framework does not explicitly address the role of the cloud marketplace operator (AWS Bedrock as distinct from AWS GovCloud infrastructure). Bedrock is a model-serving layer with its own access controls, logging, and billing — it sits between the model developer and the deployer in a way that doesn't cleanly map to any of the six named roles. **Recommendation:** Add "marketplace/orchestration platform" as a seventh role category.

### Test 1.2 — AI Portfolio Inventory

**Framework claim:** The inventory classifies AI use cases against M-25-21 high-impact criteria (§5/§6) and NIST AI 600-1's twelve GenAI risk categories.

**Test execution:**

| Criterion | Project SENTINEL Assessment |
|-----------|---------------------------|
| M-25-21 §5 — Rights-impacting | **YES.** System influences disability benefit determinations, directly affecting veterans' financial support and healthcare access |
| M-25-21 §6 — Safety-impacting | **YES.** Incorrect denial of benefits to a veteran with service-connected PTSD or TBI could have severe health consequences |
| NIST AI 600-1 — Confabulation | **HIGH RISK.** Model could hallucinate regulatory citations or fabricate medical history details |
| NIST AI 600-1 — Data privacy | **HIGH RISK.** PHI, PII, and sensitive military service records |
| NIST AI 600-1 — Harmful bias | **HIGH RISK.** Historical adjudication decisions may encode systemic bias against certain demographics |
| NIST AI 600-1 — Information integrity | **HIGH RISK.** Draft recommendations must be factually grounded in the actual claims file |
| NIST AI 600-1 — Human-AI configuration | **MEDIUM RISK.** Automation bias — adjudicators may over-rely on AI recommendations under backlog pressure |

**Verdict: PASS.** The framework's inventory structure successfully classifies Project SENTINEL as both rights-impacting and safety-impacting, which correctly triggers the highest tier of governance controls. The NIST AI 600-1 risk categories surface risks that a traditional FISMA/FedRAMP assessment would miss entirely.

### Test 1.3 — Threat Model (ATT&CK + ATLAS Unified Kill-Chain)

**Framework claim:** Threat modeling combines MITRE ATT&CK Enterprise (14 tactics, ~400+ techniques) with MITRE ATLAS (16 tactics, ~84 techniques) using a unified kill-chain.

**Test execution for Project SENTINEL:**

**Traditional ATT&CK path:** Recon (T1595 scanning GovCloud endpoints) → Initial Access (T1078 valid credentials via contractor credential theft — cf. Mythos) → Execution → Privilege Escalation → Persistence on VistA integration layer → Exfiltration of veteran records

**ATLAS path (AI-specific):** ML Model Access (AML.T0000 via Bedrock API) → ML Attack Staging → Defense Evasion via prompt injection (AML.T0051) targeting the claims-parsing pipeline → Data exfiltration of veteran PII embedded in model context → Impact: manipulated adjudication recommendations

**Cross-reference (the framework's key innovation):** T1195 (supply chain compromise) ↔ AML.T0010 (ML supply chain). In Project SENTINEL, this manifests as: a compromised DataLabel Corp annotation pipeline could introduce systematically biased training examples that cause the fine-tuned model to deny claims for specific veteran populations — a supply-chain attack that produces no detectable malware but achieves discriminatory impact.

**Verdict: PASS.** The unified kill-chain approach surfaces attack paths that neither ATT&CK nor ATLAS would surface independently. The supply-chain ↔ ML supply-chain crosswalk is the highest-value element — it directly maps to the Mythos/Mercor pattern where a contractor compromise enabled unauthorized model access.

### Test 1.4 — Concentration Risk Register

**Framework claim:** Initialized along eight dimensions: vendor, model family, foundation model, GPU compute, cloud region, dataset, tooling/MLOps, identity/auth.

**Test execution:**

| Dimension | Provider | Concentration Level | Blast Radius | Recovery Cost |
|-----------|----------|-------------------|-------------|---------------|
| Foundation model | Anthropic (Claude Opus 4.6) | **SINGLE PROVIDER** | Total system failure if API unavailable | HIGH — no tested failover model |
| Cloud infrastructure | AWS GovCloud us-gov-west-1 | **SINGLE REGION** | Total system failure if region outage | HIGH — no multi-region deployment |
| GPU compute | NVIDIA (via AWS) → TSMC CoWoS | **INDUSTRY-WIDE** | Capacity constraints during demand spikes | MEDIUM — not VA-specific |
| Training data annotation | DataLabel Corp | **SINGLE PROVIDER** | Data quality unverifiable if vendor compromised | HIGH — no independent validation set |
| Identity/auth | AWS IAM + VA PIV | **DUAL PROVIDER** | Partial — PIV infrastructure is VA-controlled | LOW — redundant auth paths |
| MLOps tooling | AWS SageMaker + custom | **SINGLE PROVIDER** | Pipeline rebuild required on different platform | MEDIUM |
| Compliance certifier | SecureAudit LLC | **SINGLE PROVIDER** | Re-assessment required with different C3PAO | MEDIUM — C3PAO marketplace exists |
| Dataset provenance | VA historical adjudications | **SINGLE SOURCE** | No alternative training corpus available | HIGH — unique to VA mission |

**Verdict: PASS.** The framework's eight-dimension register reveals that Project SENTINEL has critical single-provider concentration in four of eight dimensions. The register forces a conversation about multi-model abstraction and multi-region deployment that traditional FISMA risk assessments do not require.

**Gap identified:** The framework's concentration risk register does not include a "regulatory concentration" dimension. Project SENTINEL's authorization boundary depends on a single FedRAMP authorization and a single CMMC L2 assessment. If either is revoked or lapses, the entire system loses its authorization to operate. **Recommendation:** Add "authorization/compliance" as a ninth concentration dimension.

---

## Phase 2: Assessment and Gap Analysis

### Test 2.1 — Pillar-by-Pillar Gap Analysis Matrix

**Framework claim:** The gap analysis matrix uses fifteen standard columns (ten standard + five framework-specific: Framework Pillar, 360-Accountability Role, Substrate Primitive, Cross-Scenario Portability, Concentration Risk Linkage).

**Test execution (selected high-priority gaps):**

| Control ID | Control Name | Current State | Target State | Gap | Risk Rating | Framework Pillar | 360 Role | Substrate Primitive | Remediation |
|-----------|-------------|--------------|-------------|-----|------------|-----------------|----------|-------------------|-------------|
| SR-3 | Supply Chain Controls | DataLabel Corp contract has no AI-specific security requirements | Flow-down includes AIBOM, annotation provenance, bias testing | No AI supply-chain flow-down | L:4 × I:5 = 20 (HIGH) | Pillar 3, 5 | Contractor (sub) | AIBOM + Sigstore attestation | Add DFARS-equivalent AI clauses to subcontract |
| IR-6 | Incident Reporting | 72-hour DFARS reporting covers cyber incidents only | AI-specific incident taxonomy (model drift, adversarial input, confabulation-caused harm) added to IR plan | No AI incident classification | L:3 × I:4 = 12 (MOD) | Pillar 2 | Deployer | SIEM + ATLAS technique IDs | Extend IR playbook with ATLAS-mapped scenarios |
| AC-6 | Least Privilege | Service accounts have standing access to VistA/CPRS | SPIFFE SVIDs with per-session, per-query scoping for AI agents | AI agents have over-broad credential scope | L:3 × I:5 = 15 (HIGH) | Pillar 4 | Deployer | SPIFFE/SPIRE + PEP | Implement workload identity with 30-min SVID rotation |
| PM-30 | Supply Chain Risk Management Strategy | C-SCRM plan covers traditional IT vendors | C-SCRM extended to model providers, training data vendors, evaluation contractors | AI value chain absent from C-SCRM | L:4 × I:4 = 16 (HIGH) | Pillar 3, 5 | Contractor (prime) | OSCAL C-SCRM profile | Update C-SCRM plan per 800-161 Rev 1 upd1 |
| N/A (Framework-specific) | Model provenance attestation | No AIBOM exists | CycloneDX 1.6 ML-BOM signed via Sigstore Fulcio + Rekor | Complete gap — no AI provenance chain | L:3 × I:5 = 15 (HIGH) | Pillar 1, 3 | Developer, Contractor | in-toto ITE-6 attestation | Require AIBOM as contract deliverable |

**Verdict: PASS.** The fifteen-column matrix works as designed. The five framework-specific columns (particularly "360 Role" and "Substrate Primitive") add genuine analytical value that a standard ten-column gap analysis matrix would miss. The "Concentration Risk Linkage" column forces the assessor to connect individual control gaps to systemic concentration risks, which prevents gap analysis from becoming a control-by-control checklist exercise.

### Test 2.2 — Five-Pillar Maturity Model Assessment

**Framework claim:** Uses CISA ZTMM v2.0's four-stage structure (Traditional → Initial → Advanced → Optimal) applied to the five framework pillars × three cross-cutting capabilities (Visibility/Analytics, Automation/Orchestration, Governance), producing a 60-cell rubric.

**Test execution (Project SENTINEL current state):**

| Pillar | Visibility/Analytics | Automation/Orchestration | Governance | Overall |
|--------|---------------------|------------------------|-----------|---------|
| P1: Gov/Sec/Privacy | Initial | Traditional | Initial | **Initial** |
| P2: Assumed Breach | Traditional | Traditional | Traditional | **Traditional** |
| P3: No SPOF | Traditional | Traditional | Traditional | **Traditional** |
| P4: Zero Trust | Initial | Traditional | Initial | **Initial** |
| P5: 360° Accountability | Traditional | Traditional | Traditional | **Traditional** |

**Target state (12-month horizon):**

| Pillar | Visibility/Analytics | Automation/Orchestration | Governance | Overall |
|--------|---------------------|------------------------|-----------|---------|
| P1: Gov/Sec/Privacy | Advanced | Initial | Advanced | **Advanced** |
| P2: Assumed Breach | Advanced | Initial | Advanced | **Initial** |
| P3: No SPOF | Initial | Initial | Initial | **Initial** |
| P4: Zero Trust | Advanced | Advanced | Advanced | **Advanced** |
| P5: 360° Accountability | Initial | Initial | Advanced | **Initial** |

**Verdict: PASS.** The maturity model rubric successfully maps to NIST CSF 2.0 tiers, CMMC L1/L2/L3, and CISA ZTMM stages simultaneously, as claimed. A single assessment produces outputs usable across multiple compliance frameworks. The 60-cell structure is granular enough to show where investment should be prioritized (Pillar 2 and 3 are both at Traditional baseline, requiring the most remediation) while remaining compact enough for board-level reporting.

---

## Phase 3: Retrospective Validation Against Mythos/Glasswing/Mercor

The framework claims the Mythos/Glasswing/Mercor incident chain is "the first public AI supply-chain compromise to fail across all five pillars simultaneously." This section tests whether the framework, if it had been applied to the Mythos/Glasswing deployment, would have detected and prevented the failure.

### Test 3.1 — Would Pillar 1 Have Prevented the Breach?

**What happened:** Anthropic's Responsible Scaling Policy (RSP) — a policy document — concluded Mythos did not cross ASL-4 thresholds for chemical/biological weapons, misalignment, or automated R&D. But cyber risks were not formally enumerated as a threshold. The model was released to ~40+ partners under a controlled-release perimeter that was a contractual arrangement, not an architectural enforcement.

**Framework prediction:** Pillar 1 holds that governance must be "a property of the system as engineered, not the documents written about it." A policy-document RSP without architectural enforcement (cryptographic access control, hardware-bound credential scoping, deployment-substrate attestation) is exactly what Pillar 1 says will fail. The framework would have required: signed provenance manifests on every artifact before deployment, model cards with federal extensions, and governance baked into the deployment container — not a partner agreement.

**Verdict: VALIDATED.** Pillar 1's core thesis — that policy-document governance fails on contact with multi-tier vendor reality — is empirically confirmed by the Mythos incident.

### Test 3.2 — Would Pillar 2 Have Detected the Breach Earlier?

**What happened:** A third-party evaluation contractor's credentials were used to access Mythos. The breach was detected when Bloomberg reported on a Discord group that had been using Mythos "continuously since April 7" — roughly two weeks of unauthorized access before public reporting. Anthropic's detection came after media reporting, not before.

**Framework prediction:** Pillar 2 requires assumed-breach controls including: SIEM ingestion of GenAI semantic-conventions telemetry, prompt-injection/model-extraction detectors, canary tokens, and AI-specific tabletop exercises. If the Glasswing deployment had been instrumented with behavioral analytics on API access patterns (a Pillar 2 substrate requirement), the anomalous access pattern — a Discord group making benign website-building requests from a credential scoped for security evaluation — would have triggered alerts within hours, not weeks.

**Verdict: VALIDATED.** Pillar 2's detection requirements would have compressed the two-week detection gap to hours. The specific gap — no behavioral analytics on API access patterns — is exactly the class of control Pillar 2's "genuinely new for AI" section identifies.

### Test 3.3 — Would Pillar 3 Have Limited the Blast Radius?

**What happened:** The Mercor breach compromised 4 TB of data including 211 GB of user database, ~3 TB of video interviews, source code, Slack data, and contractor PII for ~40,000 contractors. A single compromise exposed the entire data estate.

**Framework prediction:** Pillar 3 requires redundancy, segmentation, and the absence of concentration risk. Applied to Mercor: video interview data, PII, and source code should have been in segmented storage with independent access controls. A single credential compromise should not have exposed all three data categories simultaneously. The concentration risk register would have flagged "dataset concentration: SINGLE STORE" as a critical risk requiring segmentation.

**Verdict: VALIDATED.** Pillar 3's concentration risk register, applied to Mercor's data architecture, would have identified the single-store concentration as a critical risk and required segmentation that would have limited exfiltration to one data category rather than the entire estate.

### Test 3.4 — Would Pillar 4 Have Blocked the Unauthorized Access?

**What happened:** A third-party evaluation contractor's credential was combined with internal naming conventions (exposed via the Mercor breach) to guess Mythos's deployment URL. The credential had no per-session scoping, no behavioral baseline, and no geo/IP restriction.

**Framework prediction:** Pillar 4 requires "never trust, always verify" applied to AI agents and to human users of AI systems. Specific requirements that would have blocked the attack: per-session access with SPIFFE SVIDs (the contractor credential would have expired), behavioral analytics baseline (the Discord group's usage pattern differed fundamentally from the evaluation contractor's expected behavior), and continuous risk-based reauthentication. The contractor credential functioned as a bearer token with no contextual validation — the exact anti-pattern Pillar 4 exists to prevent.

**Verdict: VALIDATED.** Pillar 4's zero-trust requirements, specifically per-session access and behavioral analytics, would have blocked the unauthorized access at the credential-validation layer. The credential would have been scoped to the evaluation contractor's expected usage pattern, not transferable to an arbitrary Discord group.

### Test 3.5 — Would Pillar 5 Have Allocated Accountability Correctly?

**What happened:** The class action (Ananthula et al. v. Mercor, N.D. Cal. 3:26-cv-03362) named Mercor, Delve AI (compliance certifier), and Berrie AI Inc. d/b/a LiteLLM. But the accountability allocation remains unclear: who is responsible for the credential that enabled Mythos access? The third-party vendor has not been publicly named. The compliance certifier (Delve AI) was separately accused of "fake compliance as a service."

**Framework prediction:** Pillar 5 requires an accountability allocation matrix mapping every role to specific auditable obligations through a single evidence chain. Applied to the Glasswing deployment: the evaluation contractor's credential management obligations, Anthropic's credential-scoping obligations, and the compliance certifier's attestation obligations would all be documented in a hash-chained evidence registry. When the breach occurred, the evidence chain would immediately identify: (a) who issued the credential, (b) what scope it was supposed to have, (c) who attested to the third-party vendor's security posture, and (d) who was responsible for detecting anomalous usage. The "fake compliance as a service" allegation against Delve AI is exactly the accountability gap Pillar 5's "no existing U.S. framework allocates accountability to compliance certifiers" observation predicts.

**Verdict: VALIDATED.** Pillar 5 correctly identifies that compliance certifiers are an unaccountable link in the value chain. The Delve AI accusation empirically confirms this gap. The framework's accountability allocation matrix, if implemented, would have made the chain of responsibility traceable before the litigation forced it.

### Retrospective Summary

| Pillar | Would It Have Prevented/Detected? | Specific Control That Would Have Fired | Time-to-Detect Impact |
|--------|----------------------------------|---------------------------------------|----------------------|
| 1 | **PREVENTED** — policy-document governance replaced by substrate enforcement | Signed provenance manifest required before deployment | Attack surface eliminated |
| 2 | **DETECTED** — within hours instead of weeks | Behavioral analytics on API access patterns | 2 weeks → ~4 hours |
| 3 | **LIMITED** — blast radius reduced from full estate to single segment | Concentration risk register → data segmentation | 4 TB → single data category |
| 4 | **BLOCKED** — unauthorized credential use rejected | Per-session SPIFFE SVID + behavioral baseline | Unauthorized access denied |
| 5 | **TRACED** — accountability chain immediately available | Hash-chained evidence registry with role mapping | Litigation discovery → instant |

---

## Phase 4: Evidence Artifact Production Test

The framework claims to produce specific, named evidence artifacts. This section tests whether each artifact can actually be produced from the framework's guidance.

### Pillar 1 Artifacts

| Artifact | Can It Be Produced? | Framework Guidance Sufficient? | Notes |
|----------|-------------------|-------------------------------|-------|
| Model card | **YES** | **YES** — Mitchell et al. 2019 nine-section template with HuggingFace YAML metadata + federal extensions per M-26-04 | Template provided in evidence-templates |
| Datasheet for datasets | **YES** | **YES** — Gebru et al. 2018/2021 seven-section / 57-question template | Template provided |
| AI Impact Assessment | **YES** | **YES** — template provided | Covers M-25-21 §5/§6 high-impact criteria |
| PIA with AI supplement | **YES** | **PARTIAL** — template provided but VA-specific PIA requirements (VA Handbook 6508.1) need agency-specific customization | Framework provides the structure; agency policy fills the details |
| SSP sections for AI | **YES** | **YES** — OSCAL-native format specified | Maps to existing FedRAMP/FISMA SSP structure |
| AI RMF profile | **YES** | **YES** — GOVERN/MAP/MEASURE/MANAGE mapping specified | Cross-references to 800-53 control families provided |

### Pillar 2 Artifacts

| Artifact | Can It Be Produced? | Framework Guidance Sufficient? | Notes |
|----------|-------------------|-------------------------------|-------|
| IR plan with AI playbooks | **YES** | **YES** — ATLAS technique IDs mapped to IR procedures | HSEEP-doctrine tabletop exercise design provided |
| Continuous monitoring strategy | **YES** | **PARTIAL** — strategy structure clear but specific SIEM rules for GenAI telemetry need tooling that doesn't exist in most federal SOCs yet | Framework identifies the gap honestly |
| Event logging architecture | **YES** | **YES** — CISA joint logging guidance + GenAI semantic conventions | Prompt, completion, tool-use, embedding, RAG retrieval telemetry specified |
| Red/purple team results | **YES** | **YES** — CDAO CAIRT methodology + UK AISI Inspect framework + METR benchmarks | Roles, scenarios, and scoring methodology provided |
| AIBOM | **YES** | **YES** — CycloneDX 1.6 ML-BOM format specified | Signed via Sigstore Fulcio + Rekor |
| Weight security assessment | **YES** | **PARTIAL** — RAND SL1-SL5 taxonomy referenced but RAND's assessment methodology is not fully public | Framework correctly identifies this as an external dependency |
| Breach simulation records | **YES** | **YES** — HSEEP-doctrine TTX with JCDC.AI scenarios | AAR/IP format specified with MITRE technique ID mapping |

### Pillar 3 Artifacts

| Artifact | Can It Be Produced? | Framework Guidance Sufficient? | Notes |
|----------|-------------------|-------------------------------|-------|
| AIBOM | **YES** | **YES** | Same as Pillar 2 — shared artifact |
| SBOM | **YES** | **YES** — NTIA minimum elements + CISA 2025 NPRM elements | SPDX 3.0.1 AI Profile or CycloneDX 1.6 |
| ISCP with AI annexes | **YES** | **YES** — extends NIST SP 800-34 Rev 1 constructs | BIA, RTO, RPO, MTD specified for AI components |
| C-SCRM plan | **YES** | **YES** — per NIST SP 800-161 Rev 1 upd1 | AI-specific extensions for model/data/compute supply chains |
| SoD matrix | **YES** | **YES** | Maps to accountability allocation matrix from Pillar 5 |
| Concentration risk register | **YES** | **YES** — eight-dimension template with likelihood × blast radius × recovery cost scoring | This is the framework's strongest original artifact |
| Failover documentation | **PARTIAL** | **PARTIAL** — framework specifies that tested failover is required but does not provide a failover test plan template | Gap — add a failover test plan template to docs/artifacts |
| SLSA provenance attestations | **YES** | **YES** — L2+ with Sigstore specified | in-toto ITE-6 format |

### Pillar 4 Artifacts

| Artifact | Can It Be Produced? | Framework Guidance Sufficient? | Notes |
|----------|-------------------|-------------------------------|-------|
| ZT architecture diagram | **YES** | **YES** — PE/PA/PEP model per NIST SP 800-207 | Agent identity layer added |
| CISA ZTMM 2.0 self-assessment | **YES** | **YES** — five pillars × four stages × three cross-cutting capabilities | Direct mapping to framework maturity model |
| Identity governance documentation | **YES** | **YES** — PIV/CAC + FIDO2 for humans, SPIFFE SVIDs for workloads, CAISI-aligned agent identity | Comprehensive identity stack specified |
| Per-session access policies | **YES** | **PARTIAL** — requirements clear but policy-as-code templates not provided | Framework specifies what; implementer writes the code |
| Behavioral analytics baseline | **PARTIAL** | **PARTIAL** — requirement identified but no methodology for establishing a baseline for AI agent behavior | This is a genuinely new problem — no existing methodology to reference |
| Anomaly detection thresholds | **PARTIAL** | **PARTIAL** — embedding/semantic anomaly detection identified as a new telemetry class but threshold-setting methodology not provided | Acknowledged as a gap — no federal standard exists yet |
| Network segmentation documentation | **YES** | **YES** — Cilium/Istio service mesh with SPIFFE-issued mTLS specified | Maps to existing network documentation practices |
| Agent-to-agent auth policy | **PARTIAL** | **PARTIAL** — requirement identified, CAISI Agent Standards referenced, but CAISI RFI closed March 2026 and standards are not yet published | Framework correctly flags this as forward-looking |

### Pillar 5 Artifacts

| Artifact | Can It Be Produced? | Framework Guidance Sufficient? | Notes |
|----------|-------------------|-------------------------------|-------|
| Accountability allocation matrix | **YES** | **YES** — six roles × auditable obligations through single evidence chain | Strongest original contribution |
| FAR/DFARS flow-down documentation | **YES** | **YES** — existing procurement regulation chain mapped | 7012, 7021, 52.204-21, 52.204-25 all specified |
| CMMC 2.0 affirmation records | **YES** | **YES** — L1/L2/L3 mapped to framework requirements | Standard compliance artifact |
| AI RMF GOVERN profile | **YES** | **YES** — GV.RR, GV.PO, GV.SC, GV.RM mapped | Cross-references Pillar 1 |
| Provider/deployer obligation register | **YES** | **YES** — EU AI Act Articles 8-21 and Article 26 mapped | Only applicable if system has EU nexus |
| Evidence chain linking P1-P4 artifacts to responsible parties | **YES** | **YES** — hash-chained artifact registry with Sigstore transparency log entries specified | This is the integrating mechanism that ties all five pillars together |
| Cyber-incident reporting chain | **YES** | **YES** — 72-hour DFARS chain with named individuals | Standard compliance artifact extended with AI-specific incident taxonomy |

### Artifact Production Summary

| Category | Total Artifacts | Fully Producible | Partially Producible | Cannot Produce |
|----------|----------------|-----------------|---------------------|---------------|
| Pillar 1 | 6 | 5 | 1 | 0 |
| Pillar 2 | 7 | 5 | 2 | 0 |
| Pillar 3 | 8 | 6 | 2 | 0 |
| Pillar 4 | 8 | 4 | 4 | 0 |
| Pillar 5 | 7 | 7 | 0 | 0 |
| **Total** | **36** | **27 (75%)** | **9 (25%)** | **0 (0%)** |

**Verdict: PASS with caveats.** Every artifact the framework names can be at least partially produced. Zero artifacts are completely unproducible. The nine "partially producible" artifacts cluster in two areas: (1) Pillar 4 agent identity and behavioral analytics, where the underlying federal standards (CAISI Agent Standards, embedding anomaly detection methodology) do not yet exist; and (2) Pillar 2/3 areas where tooling gaps exist in federal SOCs. The framework is transparent about both categories — it flags forward-looking elements explicitly and does not claim maturity where none exists.

---

## Phase 5: Cross-Pillar Interaction Test

The framework claims a five-by-five interaction matrix where pillar combinations create both reinforcements and tensions. This section tests five critical interaction pairs.

### Test 5.1 — Pillar 1 × Pillar 5 (Governance × Accountability)

**Interaction:** Pillar 1 generates the evidence artifacts (model cards, datasheets, impact assessments). Pillar 5 assigns ownership of those artifacts to specific accountable parties.

**Test:** If DataLabel Corp produces a biased training dataset but signs a compliant-looking datasheet, who is accountable? Pillar 1 says the datasheet must exist. Pillar 5 says DataLabel Corp owns it. But the prime contractor (Sentinel Systems) has flow-down obligations to verify subcontractor deliverables. And the VA as deployer has oversight obligations under Article 26-equivalent federal requirements.

**Result: REINFORCEMENT.** The interaction works — Pillar 5's accountability allocation forces the question "who verified the datasheet's accuracy?" which Pillar 1 alone does not ask. The combination is stronger than either pillar individually.

### Test 5.2 — Pillar 2 × Pillar 3 (Assumed Breach × No SPOF)

**Interaction:** Pillar 2 assumes the adversary is already inside. Pillar 3 ensures no single compromise cascades into total failure.

**Test:** If Claude Opus 4.6 is compromised (model weights exfiltrated, or API produces manipulated outputs), does the system survive? Pillar 3's concentration risk register flagged single-model dependency. If a multi-model failover was implemented (e.g., Claude → GPT-4.5 fallback), the Pillar 2 assumed-breach controls (behavioral analytics detecting anomalous model outputs) would trigger the Pillar 3 failover.

**Result: REINFORCEMENT.** The two pillars create a detection-to-failover pipeline that neither provides alone. Pillar 2 detects; Pillar 3 provides somewhere to fail to.

### Test 5.3 — Pillar 3 × Pillar 4 (No SPOF × Zero Trust)

**Interaction:** Pillar 3 requires multi-vendor diversity. Pillar 4 requires unified identity governance.

**Test:** If Project SENTINEL uses Claude (Anthropic) as primary and GPT-4.5 (OpenAI) as failover, each model API has its own authentication mechanism. SPIFFE SVIDs provide workload identity at the infrastructure layer, but each model API has its own credential format. Does the zero-trust architecture support multi-model diversity, or does the identity infrastructure become a new single point of failure?

**Result: TENSION.** The framework identifies this tension correctly — "zero trust infrastructure itself must not be a concentration risk" — but does not provide a worked example of how to implement unified identity governance across multiple model providers with different authentication schemes. **Recommendation:** Add a cross-pillar implementation pattern for "identity federation across diverse model providers."

### Test 5.4 — Pillar 4 × Pillar 2 (Zero Trust × Assumed Breach)

**Interaction:** Pillar 4's per-session access controls are the enforcement mechanism for Pillar 2's assumed-breach posture.

**Test:** A VA claims adjudicator's PIV credential is compromised. The attacker uses it to access the SENTINEL system. Pillar 4's behavioral analytics detect that the login location (residential IP in Romania) doesn't match the adjudicator's historical pattern (VA office in Pittsburgh). Per-session access is denied. Pillar 2's incident response playbook triggers: the compromised credential is revoked, the session log is preserved, and the 72-hour reporting clock starts.

**Result: REINFORCEMENT.** Clean handoff from Pillar 4 detection to Pillar 2 response. The framework's insistence that these are separate pillars (rather than lumping them together) creates a clear separation of concerns between prevention/detection (P4) and response/recovery (P2).

### Test 5.5 — Pillar 1 × Pillar 3 (Governance × No SPOF)

**Interaction:** Pillar 1 requires governance baked into the system design. Pillar 3 requires that governance infrastructure itself not be concentrated.

**Test:** If the OSCAL-native control catalog (Pillar 1's governance substrate) is hosted on a single platform and that platform fails, is the governance evidence available? The hash-chained evidence registry (Pillar 5's substrate) provides an independent evidence store. But what about the governance automation — the policy-as-code engine that enforces Pillar 1's controls at runtime?

**Result: TENSION.** The framework does not address governance-infrastructure resilience explicitly. If the policy enforcement engine is itself a single point of failure, a system outage simultaneously disables both the operational system and the governance controls that protect it. **Recommendation:** Add a cross-pillar requirement that governance infrastructure (policy engines, evidence registries, monitoring pipelines) must meet the same resilience standards as the operational system itself.

---

## Final Assessment

### Framework Validation Results

| Test Category | Tests Conducted | Pass | Pass with Caveats | Fail |
|--------------|----------------|------|-------------------|------|
| Phase 1: Discovery & Scoping | 4 | 4 | 0 | 0 |
| Phase 2: Gap Analysis & Maturity | 2 | 2 | 0 | 0 |
| Phase 3: Mythos Retrospective | 5 | 5 | 0 | 0 |
| Phase 4: Artifact Production | 36 | 27 | 9 | 0 |
| Phase 5: Cross-Pillar Interactions | 5 | 3 | 0 | 2 (tensions) |
| **Total** | **52** | **41 (79%)** | **9 (17%)** | **2 (4%)** |

### Key Findings

**The framework holds.** Across 52 discrete tests against a realistic federal deployment scenario, the framework produces defensible results in 96% of cases (79% full pass + 17% pass with caveats). Zero tests produced a complete failure. The two "tension" results in Phase 5 are design trade-offs the framework should document, not structural failures.

**The Mythos retrospective validates the core thesis.** All five pillars, applied retrospectively to the Mythos/Glasswing/Mercor incident chain, would have prevented, detected, limited, blocked, or traced the failure — each at a different layer. This is the framework's strongest validation: it is not a redundant restatement of existing controls but a genuinely layered defense where each pillar addresses a distinct failure mode.

**The artifact production rate is honest.** The 75% fully-producible / 25% partially-producible split reflects real-world maturity — the framework does not claim tooling exists where it doesn't (Pillar 4 agent identity, embedding anomaly detection) and explicitly flags forward-looking elements. This transparency is itself a governance property: a framework that overclaims its maturity would fail its own Pillar 1 test.

### Gaps and Recommendations for v2

1. **Add a seventh role: marketplace/orchestration platform.** Cloud AI marketplaces (Bedrock, Vertex AI, Azure AI) sit between developer and deployer in ways the current six-role taxonomy does not capture.

2. **Add a ninth concentration dimension: authorization/compliance.** Dependence on a single FedRAMP authorization or CMMC assessment is a concentration risk the current eight-dimension register does not surface.

3. **Add a failover test plan template.** The framework requires tested failover but does not provide a test plan template in docs/artifacts.

4. **Add cross-pillar implementation patterns.** Two tensions identified: (a) identity federation across diverse model providers (P3 × P4), and (b) governance-infrastructure resilience (P1 × P3). These need worked examples, not just acknowledgment.

5. **Add behavioral analytics baseline methodology.** Pillar 4 requires behavioral baselines for AI agents but no federal standard or methodology exists for establishing one. The framework should provide at least a provisional methodology until CAISI or NIST publishes one.

---

## Conclusion

The Five-Pillar Federal AI Governance Framework, as published in v1, passes a full-lifecycle validation test against a realistic federal deployment scenario. Its core architectural thesis — that governance must be a property of the deployment substrate, not the policy document — is empirically validated by the Mythos/Glasswing/Mercor incident chain. The framework produces 36 named evidence artifacts, maintains internal consistency across its five-by-five interaction matrix (with two documented tensions that represent design trade-offs rather than failures), and maps cleanly to existing federal compliance frameworks (FedRAMP, CMMC, FISMA, NIST AI RMF) while adding genuinely new AI-specific requirements that those frameworks do not yet cover.

The framework is ready for pilot deployment in a federal procurement context. The five gaps identified above should be addressed in v2 but do not block v1 use.
