# Product Requirements Document
## RegRadar: AI-Powered Regulatory Change Management
**Version**: 1.0  
**Status**: Concept / Portfolio  
**Author**: Lois Anum 
**Last Updated**: February 2026

---

## Table of Contents
1. [Executive Summary](#1-executive-summary)
2. [Problem Statement](#2-problem-statement)
3. [User Research](#3-user-research)
4. [Goals & Success Metrics](#4-goals--success-metrics)
5. [Solution Overview](#5-solution-overview)
6. [AI Architecture & Design Decisions](#6-ai-architecture--design-decisions)
7. [Functional Requirements](#7-functional-requirements)
8. [Non-Functional Requirements](#8-non-functional-requirements)
9. [User Stories & Acceptance Criteria](#9-user-stories--acceptance-criteria)
10. [Risk Register](#10-risk-register)
11. [Phased Roadmap](#11-phased-roadmap)
12. [Out of Scope](#12-out-of-scope)
13. [Open Questions](#13-open-questions)

---

## 1. Executive Summary

RegRadar is an AI-powered regulatory change management platform designed for UK-regulated financial institutions. It uses Retrieval-Augmented Generation (RAG) to automatically monitor regulatory publications from the FCA, PRA, and Bank of England; identify affected internal policies; and generate structured, human-reviewable impact assessments — reducing analyst triage time by an estimated 70% while maintaining the full audit trails required in regulated environments.

The product addresses a genuine and costly gap: despite banks investing heavily in compliance functions, the core process of reading, interpreting, and triaging regulatory change remains manual, slow, and inconsistent. RegRadar does not automate compliance decisions — it eliminates the low-value reading and cross-referencing work so that expert analysts can focus on judgment, escalation, and remediation.

---

## 2. Problem Statement

### 2.1 Background

UK financial institutions operate under a complex and continuously evolving regulatory framework. The FCA alone issued over 900 publications in 2023, ranging from policy statements and consultation papers to supervisory letters and finalised guidance. Each publication must be reviewed to determine whether it affects the bank's products, processes, policies, or risk profile.

The current process at most banks follows a broadly similar pattern:

1. A compliance analyst (or team) manually monitors regulator websites and email alerts
2. Relevant documents are downloaded, read, and summarised manually
3. The analyst cross-references internal policy documents to identify what needs updating
4. An impact assessment is drafted and circulated to relevant stakeholders via email
5. Sign-off is collected — often informally — and the policy update is scheduled

### 2.2 Core Problems

**Speed**: A non-trivial regulatory update — such as a new FCA Consumer Duty guidance paper — can take a skilled analyst 2–3 weeks to fully triage across all affected policies. Meanwhile, implementation deadlines do not flex.

**Inconsistency**: The quality and thoroughness of an impact assessment depends on the individual analyst's experience, familiarity with the firm's policy library, and current workload. There is no standardised output format.

**Auditability**: Impact assessments typically live in email threads and shared drives. If a regulator later asks to see evidence of a firm's response to a particular publication, reconstructing that audit trail is painful and unreliable.

**Latency**: Regulatory monitoring is often reactive. Junior analysts may not flag a relevant publication until it has been in the public domain for weeks, compressing implementation timelines.

**Scale**: Post-Brexit, UK banks now face divergent UK and EU regulatory trajectories. The volume and complexity of regulatory change is increasing, not decreasing.

### 2.3 Problem Statement

> Compliance and policy teams at UK banks spend a disproportionate amount of highly skilled analyst time on low-value reading and cross-referencing tasks, resulting in slow triage cycles, inconsistent output quality, and inadequate audit trails — at increasing risk as regulatory volume grows.

---

## 3. User Research

### 3.1 Primary Users

**Compliance Analyst (Core User)**
- Responsible for monitoring regulatory publications, assessing impact, and maintaining the internal policy library
- Typically 2–8 years experience; strong legal/regulatory knowledge
- Current pain: spends 40–60% of time on reading and document comparison tasks they describe as "necessary but not the job I was hired to do"
- Concern about AI: fears being replaced; needs to feel the tool supports their expertise rather than undermining it

**Head of Regulatory Affairs / Chief Compliance Officer (Approver)**
- Responsible for the firm's overall regulatory posture; signs off on material impact assessments
- Current pain: lacks real-time visibility into what's been triaged, what's in progress, and what's overdue
- Concern about AI: regulatory liability — if an AI assessment is wrong and the firm misses a compliance obligation, who is accountable?

**Policy Owner (Downstream User)**
- Business line owner who is notified when a policy in their area requires updating
- Current pain: receives ad hoc email requests with unclear urgency and insufficient context
- Needs: clear, structured notification with enough context to understand what action is required and by when

### 3.2 User Needs Summary

| User | Primary Need | Key Fear |
|------|-------------|----------|
| Compliance Analyst | Reduce time on mechanical reading/comparison | Being blamed for AI errors |
| Head of Compliance / CCO | Real-time visibility and audit-ready records | Regulatory liability for AI-assisted decisions |
| Policy Owner | Clear, contextualised action requests | Being overwhelmed with false positives |

### 3.3 Research Assumptions (To Be Validated)

The following assumptions are based on industry knowledge and would require validation through user interviews in a real discovery phase:

- Analysts spend >40% of their time on document reading and comparison tasks that could be assisted by AI
- Current tooling (email, shared drives, spreadsheet trackers) is the dominant workflow at mid-to-large UK banks
- Analysts would trust AI-generated assessments if outputs clearly cite source documents and are labelled as requiring human review
- CCOs would accept AI assistance if every output has a named human sign-off and a full audit log

---

## 4. Goals & Success Metrics

### 4.1 Product Goals

| Goal | Description |
|------|-------------|
| Reduce triage time | Cut the time from regulatory publication to completed first-pass impact assessment |
| Improve consistency | Standardise output format and coverage across all publications |
| Strengthen auditability | Create an immutable, searchable record of all assessments and approvals |
| Increase analyst satisfaction | Free analysts from mechanical tasks so they can focus on judgment work |

### 4.2 Primary Success Metrics (OKR Format)

**Objective: Meaningfully accelerate regulatory change triage without compromising quality or compliance**

| Key Result | Baseline (Estimated) | Target (12 months) |
|-----------|---------------------|-------------------|
| Median time from publication to first-pass impact assessment | 5 business days | ≤1 business day |
| % of publications triaged within SLA | ~60% | ≥90% |
| Analyst time spent on reading/comparison tasks | ~50% of role | ≤20% of role |
| % of impact assessments with complete audit trail | ~40% | 100% |
| Analyst NPS | Unknown | ≥40 |

### 4.3 Guardrail Metrics (Must Not Degrade)

- False negative rate (missed material impacts): must remain below 2% — this is the critical safety metric
- Regulatory findings attributable to missed change management: 0 increase year-on-year
- Time to human sign-off on AI assessments: must not exceed current total triage time (no net slowdown)

---

## 5. Solution Overview

### 5.1 Core Workflow

```
[Regulatory Feed Ingestion]
        ↓
[Document Classification & Relevance Scoring]
        ↓
[Semantic Search Against Internal Policy Library]
        ↓
[AI Draft Impact Assessment Generation]
        ↓
[Analyst Review Interface — Human-in-the-Loop]
        ↓
[Approval, Sign-off & Audit Log]
        ↓
[Policy Owner Notification & Task Assignment]
```

### 5.2 Key Features

**Regulatory Feed Monitor**
Automated ingestion of publications from FCA, PRA, Bank of England, and configurable additional sources (EBA, ESMA for EU-relevant firms). New publications are detected within 1 hour of release.

**Relevance Classification**
An LLM-based classifier scores each publication for relevance to the firm's activities before any deeper analysis is triggered. This prevents analyst queues from being flooded with irrelevant publications.

**Policy Impact Mapper**
For relevant publications, a RAG pipeline retrieves semantically similar sections from the internal policy library and identifies specific policies that may require review. Each mapping includes the specific clauses from both documents that drove the match.

**AI Impact Assessment Drafter**
The system generates a structured draft impact assessment in a standardised format: summary of regulatory change, affected policies, required actions, suggested priority, and confidence level. All outputs are clearly labelled as AI-generated drafts.

**Analyst Review Interface**
A clean web interface where analysts can review, edit, annotate, and approve AI-generated assessments. All edits are tracked. The interface shows the AI's reasoning — which clauses it matched and why — so analysts can verify rather than blindly accept.

**Audit & Reporting Dashboard**
An immutable log of every publication ingested, every assessment generated, every human edit, and every sign-off — with timestamps and named individuals. Exportable for regulatory review. A dashboard gives CCOs real-time visibility into the pipeline.

---

## 6. AI Architecture & Design Decisions

### 6.1 Why RAG, Not a Fine-Tuned Model

A fine-tuned model would need retraining every time the regulatory landscape changes or the internal policy library is updated — which is continuous. A RAG architecture decouples the reasoning capability (the LLM) from the knowledge store (the document database), allowing the system to retrieve from an always-current source without retraining.

Critically, RAG also enables citation: every claim in an AI-generated assessment is tied to a specific source chunk, which is essential for auditability in a regulated context.

### 6.2 Embedding & Retrieval Strategy

Internal policy documents and regulatory publications are chunked, embedded, and stored in a vector database. At query time, the incoming regulatory document is embedded and semantically searched against the policy library to retrieve the most relevant chunks.

Chunking strategy requires careful design: regulatory documents have hierarchical structure (parts, chapters, articles, paragraphs) that should be preserved in chunk boundaries to avoid context loss.

### 6.3 LLM Selection Criteria

The LLM used for assessment generation must meet the following criteria:
- **Context window**: large enough to process full regulatory documents (100k+ tokens preferred)
- **Instruction following**: reliable adherence to structured output formats (JSON schema)
- **Hallucination rate**: low on factual, document-grounded tasks — to be evaluated against internal benchmarks
- **Data residency**: must support UK/EU data residency commitments for financial services deployment

Candidate models: GPT-4o (Azure OpenAI for data residency), Claude 3 Opus (Anthropic's enterprise agreements), or an on-premise open-source model (Llama 3 variants) for maximum data control.

### 6.4 Human-in-the-Loop Design Principles

- No AI-generated assessment is ever the final record without a named human sign-off
- All AI outputs are labelled with a confidence indicator and a recommendation to verify high-risk assessments independently
- Analysts can override any AI judgement with a free-text reason, which is logged
- The system learns from human corrections over time (feedback loop into prompt refinement and retrieval tuning — not model fine-tuning, which preserves auditability)

### 6.5 Explainability Requirements

For each flagged policy match, the system must surface:
- The specific paragraph in the regulatory document that triggered the match
- The specific clause in the internal policy document that was matched
- The semantic similarity score
- A plain-English explanation of why the match was flagged

This is non-negotiable. A black-box output is not acceptable in a compliance context.

---

## 7. Functional Requirements

### 7.1 Must Have (MVP)

| ID | Requirement |
|----|-------------|
| F-01 | System ingests FCA, PRA, and Bank of England publication feeds automatically, checking for updates at least hourly |
| F-02 | Each new publication is classified for relevance (relevant / not relevant / uncertain) with a confidence score |
| F-03 | Relevant publications trigger a RAG pipeline that retrieves matching internal policy sections |
| F-04 | System generates a structured draft impact assessment in a standardised format for each relevant publication |
| F-05 | Every AI output includes citations to source documents at paragraph level |
| F-06 | Analyst review interface allows editing, annotation, and approval of AI-generated assessments |
| F-07 | All assessments require a named human sign-off before being marked as complete |
| F-08 | All actions (ingestion, AI generation, human edits, approvals) are logged to an immutable audit trail |
| F-09 | Completed assessments trigger configurable notifications to relevant policy owners |
| F-10 | Analysts can upload and manage the internal policy library, with version history maintained |

### 7.2 Should Have (Phase 2)

| ID | Requirement |
|----|-------------|
| F-11 | CCO/Head of Compliance dashboard showing pipeline status, overdue items, and trend reporting |
| F-12 | Configurable SLA alerts — escalation when a publication has not been triaged within defined thresholds |
| F-13 | Integration with policy management systems (e.g. Confluence, SharePoint) for automated policy update tracking |
| F-14 | Ability to add additional regulatory sources beyond the three core UK regulators |
| F-15 | Bulk export of audit trail in regulator-ready format |

### 7.3 Won't Have (This Version)

- Automated policy updating (AI writes the updated policy — this is explicitly out of scope; human authorship of policy changes is required)
- Consumer-facing or client-facing features
- Integration with trading or risk systems
- Any processing of personal data

---

## 8. Non-Functional Requirements

| Category | Requirement |
|----------|-------------|
| **Data Residency** | All data processed and stored within UK/EU boundaries; no transfer to US servers without explicit DPA |
| **Data Classification** | Internal policy documents treated as Confidential; no training of external models on internal data |
| **Availability** | 99.5% uptime during business hours (07:00–20:00 GMT, Monday–Friday) |
| **Latency** | AI assessment draft generated within 10 minutes of a publication being classified as relevant |
| **Audit Retention** | Audit logs retained for minimum 7 years in line with FCA record-keeping requirements |
| **Access Control** | Role-based access control (Analyst / Senior Analyst / Approver / Admin); SSO integration required |
| **Security** | SOC 2 Type II compliance; penetration tested annually; encryption at rest and in transit |
| **Explainability** | Every AI output must include source citations; black-box outputs are not acceptable |

---

## 9. User Stories & Acceptance Criteria

### Story 1: New Regulatory Publication Detected

**As a** compliance analyst,  
**I want** to be automatically alerted when a new relevant FCA publication is detected,  
**So that** I don't have to manually monitor regulator websites and can begin assessment work without delay.

**Acceptance Criteria:**
- [ ] System detects new FCA publications within 60 minutes of release
- [ ] Analyst receives an in-app notification and optional email alert
- [ ] Notification includes publication title, date, regulator, and AI-generated relevance classification with confidence score
- [ ] Publications classified as "not relevant" are visible in a separate queue for spot-checking, not discarded

---

### Story 2: AI Impact Assessment Draft Reviewed

**As a** compliance analyst,  
**I want** to review an AI-generated impact assessment that shows me which internal policies may be affected and why,  
**So that** I can focus my expertise on verifying and refining the assessment rather than starting from scratch.

**Acceptance Criteria:**
- [ ] AI draft is available in the review interface within 10 minutes of the publication being classified as relevant
- [ ] Draft includes: summary of regulatory change, list of matched internal policies, specific clauses matched (both regulatory and internal), confidence score per match, and suggested priority (High / Medium / Low)
- [ ] Each policy match displays the exact paragraphs from both documents that drove the match
- [ ] Analyst can edit any field in the draft, with all edits tracked and attributed
- [ ] Analyst can mark individual policy matches as "confirmed", "rejected", or "needs further review"

---

### Story 3: Assessment Signed Off and Audit Trail Preserved

**As a** Head of Compliance,  
**I want** every completed impact assessment to have a named sign-off and a complete, immutable record of how it was produced,  
**So that** if a regulator later asks for evidence of our change management process, I can produce it immediately.

**Acceptance Criteria:**
- [ ] No assessment can be marked as complete without a sign-off from a user with Approver role
- [ ] Audit log records: publication ingestion timestamp, AI generation timestamp, all analyst edits with before/after content, approval timestamp and approver name
- [ ] Audit log is read-only; no user can delete or modify it
- [ ] Assessment is exportable as a PDF with audit trail appended
- [ ] Audit logs are retained for 7 years

---

## 10. Risk Register

| ID | Risk | Likelihood | Impact | Mitigation |
|----|------|-----------|--------|------------|
| R-01 | AI generates a false negative (misses a material regulatory impact) | Low-Medium | Critical | Guardrail metric monitored continuously; human review required; random sampling audit of "not relevant" classifications |
| R-02 | Analysts over-trust AI outputs without critical review (automation bias) | Medium | High | UI design explicitly frames outputs as drafts; confidence scores surface uncertainty; training programme for analysts at rollout |
| R-03 | Internal policy documents contain sensitive data that is inadvertently exposed via LLM API calls | Low | Critical | Data residency controls enforced; on-premise model deployment option for highest-sensitivity firms; no PII in policy library (by design) |
| R-04 | Regulatory publications use non-standard formats (scanned PDFs, complex tables) that degrade OCR/parsing quality | Medium | Medium | Robust document parsing pipeline with quality checks; analyst flagging mechanism for poor-quality ingestion |
| R-05 | Regulator websites change their feed structure, breaking automated ingestion | Medium | Medium | Monitoring alerts on feed health; manual fallback ingestion process documented |
| R-06 | Analysts resist adoption due to fear of job displacement | Medium | High | Change management programme; reframe product as analyst augmentation; involve analysts in beta testing and iteration |
| R-07 | LLM provider has an outage or deprecates the model version in use | Low | Medium | Abstraction layer in architecture to allow model swapping; SLA monitoring; fallback to manual process documented |

---

## 11. Phased Roadmap

### Phase 1 — MVP (Months 1–4)
Core ingestion, RAG pipeline, AI assessment drafting, analyst review interface, basic audit log. Limited to FCA publications only. Piloted with a single compliance team of 5–8 analysts.

**Success gate**: Pilot team reduces average triage time by ≥50% with no increase in missed impacts.

### Phase 2 — Expand (Months 5–8)
Add PRA and Bank of England feeds. Build CCO dashboard and SLA alerting. Begin integration with SharePoint/Confluence for policy library sync. Roll out to full compliance function.

**Success gate**: ≥90% of publications triaged within 1 business day. Analyst NPS ≥40.

### Phase 3 — Scale & Optimise (Months 9–12)
Add configurable additional regulatory sources. Build bulk audit export for regulator-ready reporting. Explore integration with wider GRC (Governance, Risk & Compliance) platforms. Begin evaluation of expansion to other regulatory jurisdictions.

---

## 12. Out of Scope

- **Automated policy authoring**: The system drafts impact assessments, not updated policies. Human analysts author all policy changes.
- **Legal advice**: RegRadar does not provide legal interpretation. It surfaces potential impacts for human expert review.
- **Personal data processing**: The system processes regulatory publications and internal policies only. No customer or employee personal data is handled.
- **Automated regulatory submission**: The system does not interact with regulators on the firm's behalf.
- **Non-UK regulatory jurisdictions** (Phase 1): EU, US, and other jurisdictions are a future consideration.

---

## 13. Open Questions

| Question | Owner | Target Resolution |
|----------|-------|------------------|
| What is the firm's preferred LLM deployment model — cloud API with data residency, or on-premise? | Head of Technology / CISO | Before architecture sign-off |
| How is the internal policy library currently structured and tagged — and how mature is the metadata? | Policy Management Team | Discovery phase |
| What constitutes "material impact" — is there an existing internal threshold definition? | Head of Compliance | Before evaluation framework is finalised |
| Should the system integrate with the existing GRC platform, or operate as a standalone tool initially? | CTO / Head of Compliance | Phase 2 planning |
| What is the firm's AI governance policy, and does a model of this type require a formal AI risk assessment before deployment? | Chief Risk Officer | Before pilot launch |
