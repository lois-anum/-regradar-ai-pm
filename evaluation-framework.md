# Evaluation Framework: RegRadar
**Version**: 1.0  
**Author**: Lois Anum

---

## Overview

A product without a rigorous evaluation framework is a guess. This document defines how RegRadar would be measured across three dimensions: **AI model quality**, **product effectiveness**, and **business impact**. These are distinct and must all be tracked — a model can be technically impressive while the product fails to change analyst behaviour, or the product can be adopted while failing to deliver measurable business outcomes.

The framework is designed to reflect the unique constraints of a regulated environment, where the cost of a false negative (a missed material regulatory impact) is asymmetric — far higher than the cost of a false positive.

---

## Dimension 1: AI Model Quality

These metrics measure the performance of the underlying AI system, independent of whether users adopt the product. They are evaluated continuously in a staging environment and monitored in production.

### 1.1 Relevance Classification Accuracy

The system classifies each incoming publication as Relevant / Not Relevant / Uncertain before triggering the deeper RAG pipeline.

| Metric | Definition | Target | Measurement Method |
|--------|-----------|--------|-------------------|
| True Positive Rate (Recall) | % of genuinely relevant publications correctly classified as relevant | ≥98% | Retrospective comparison against analyst ground truth labels on a sample set |
| False Negative Rate | % of genuinely relevant publications incorrectly classified as not relevant | ≤2% | Same as above — this is the critical safety metric |
| Precision | % of publications classified as relevant that are genuinely relevant | ≥80% | Reduces analyst queue noise from false positives |
| Uncertain Rate | % classified as uncertain (requiring analyst review regardless) | Monitor — no target | Directional indicator of model confidence calibration |

**Why recall is prioritised over precision**: In this context, missing a material regulatory change (false negative) is far more dangerous than flagging an irrelevant publication (false positive). The system is calibrated to err on the side of over-flagging.

### 1.2 Policy Impact Mapping Quality

For publications classified as relevant, the system retrieves affected internal policy sections.

| Metric | Definition | Target | Measurement Method |
|--------|-----------|--------|-------------------|
| Mapping Recall | % of analyst-confirmed policy matches that the AI also identified | ≥90% | Analyst review interface tracks confirmed vs. AI-generated matches |
| Mapping Precision | % of AI-flagged policy matches that analysts confirm as genuine | ≥70% | Same as above |
| Clause Citation Accuracy | % of cited source clauses that correctly support the stated impact | ≥95% | Manual quality review of random 10% sample monthly |
| Mean Rank of Correct Match | Average rank position of analyst-confirmed matches in AI results | ≤2 | Directional quality metric |

### 1.3 Assessment Draft Quality

The quality of the AI-generated impact assessment draft.

| Metric | Definition | Target | Measurement Method |
|--------|-----------|--------|-------------------|
| Edit Rate | % of AI assessment fields that analysts materially edit before sign-off | Monitor; aim for ≤30% | Tracked automatically in review interface |
| Rejection Rate | % of AI-generated assessments entirely rejected and rewritten from scratch | ≤5% | Tracked in review interface |
| Analyst Accuracy Rating | Analyst-rated accuracy of AI draft on a 1–5 scale at point of review | ≥4.0 average | In-product rating widget |

---

## Dimension 2: Product Effectiveness

These metrics measure whether the product is achieving its intended workflow improvements, regardless of the underlying AI performance.

### 2.1 Speed Metrics

| Metric | Definition | Baseline (Estimated) | Target (12 months) |
|--------|-----------|---------------------|-------------------|
| Time to First-Pass Assessment | Time from publication release to completed analyst first-pass | 5 business days | ≤1 business day |
| Time to Signed-Off Assessment | Time from publication release to final approved assessment | 10 business days | ≤3 business days |
| Publications Triaged Within SLA | % of relevant publications with completed first-pass within 1 business day | ~40% | ≥90% |

### 2.2 Coverage Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| Publication Detection Rate | % of relevant publications detected by the system (vs. manual discovery) | ≥99% |
| Policy Library Coverage | % of internal policies indexed in the vector store | 100% before pilot launch |
| Missed Change Rate | % of regulatory changes that were not identified until after implementation deadline | 0% increase vs. baseline |

### 2.3 Audit Trail Quality

| Metric | Definition | Target |
|--------|-----------|--------|
| Assessments with Complete Audit Trail | % of completed assessments with full timestamped log from ingestion to sign-off | 100% |
| Time to Audit Pack Generation | Time to produce a regulator-ready audit pack for a given regulatory change | ≤5 minutes |
| Audit Trail Integrity Checks | Automated checks confirming log immutability | Pass rate 100% |

---

## Dimension 3: Business Impact

These metrics measure the ultimate business outcomes the product is designed to drive.

### 3.1 Analyst Productivity

| Metric | Definition | Baseline | Target (12 months) |
|--------|-----------|---------|-------------------|
| % Time on Reading/Comparison | Proportion of analyst working time spent on document reading and cross-referencing | ~50% | ≤20% |
| Assessments Completed Per Analyst Per Week | Throughput measure | Establish at pilot | 3x baseline |
| Analyst NPS | Net Promoter Score among compliance analyst user group | Unknown | ≥40 |

### 3.2 Compliance Outcomes

| Metric | Definition | Target |
|--------|-----------|--------|
| Regulatory Findings from Change Management Failures | Regulatory findings (from external exam or internal audit) attributable to missed or late regulatory change triage | 0 increase vs. prior year |
| Late Implementation Rate | % of regulatory changes where the firm identifies the need for policy change after the regulator's implementation deadline | 0% |
| Internal Audit Rating for Change Management Process | Rating from annual internal audit review of the regulatory change management process | Maintained at current rating or improved |

### 3.3 Adoption

| Metric | Definition | Target |
|--------|-----------|--------|
| Weekly Active Users | Analysts using the system at least once per week | 100% of compliance team within 3 months of full rollout |
| Feature Adoption: Citation View | % of AI assessments where the analyst views the source citation detail | ≥70% (indicator of genuine engagement vs. rubber-stamping) |
| Manual Bypass Rate | % of assessments processed outside the system (reverts to manual process) | ≤5% within 6 months of rollout |

---

## Evaluation Cadence

| Review Type | Frequency | Audience | Content |
|-------------|-----------|---------|---------|
| AI Model Quality Report | Weekly (automated) | Product team, Engineering | Model accuracy metrics; drift alerts; sample review |
| Product Metrics Review | Monthly | Product team, Head of Compliance | Speed, coverage, audit trail metrics; user feedback themes |
| Business Impact Review | Quarterly | Steering committee (CCO, CTO, CPO) | Compliance outcomes, analyst productivity, NPS, ROI estimate |
| Annual Evaluation | Annual | Executive / Board Risk Committee | Full assessment of whether the product is delivering on its business case; decision on continued investment and expansion |

---

## Evaluation of Failure Modes

A robust evaluation framework must explicitly test for the ways the product can fail, not just measure success.

### Failure Mode 1: Silent Failure (Most Critical)
The system misses a material regulatory change and no one notices until after the implementation deadline. 

**Detection approach**: Monthly retrospective review by a senior analyst of a random 10% sample of publications classified as "not relevant" — confirming the system didn't silently discard something important.

### Failure Mode 2: Automation Bias
Analysts approve AI-generated assessments without genuine review, creating the appearance of human oversight without the substance.

**Detection approach**: Monitor the edit rate and citation view rate (above). A very low edit rate combined with low citation view engagement is a warning signal that analysts are rubber-stamping. Intervention: targeted training, UI design changes to require active engagement.

### Failure Mode 3: Confidence Miscalibration
The system expresses high confidence in assessments that are actually incorrect, leading analysts to under-scrutinise them.

**Detection approach**: Calibration analysis — comparing the model's stated confidence scores against actual accuracy rates across a labelled dataset. Well-calibrated models should show ~90% accuracy on assertions made with 90% confidence.

### Failure Mode 4: Policy Library Staleness
The internal policy library is not kept current, so the RAG pipeline is matching against outdated policy versions.

**Detection approach**: Automated check of last-updated date on all policy documents in the vector store; alert when a document has not been reviewed in >12 months.

---

## Continuous Improvement Loop

RegRadar's evaluation framework feeds directly into product iteration:

1. **Weekly model review** identifies specific failure cases → prompt engineering refinements or retrieval tuning
2. **Monthly user feedback** identifies friction points → UX improvements in the review interface
3. **Quarterly business review** assesses whether the product is delivering ROI → investment and roadmap decisions
4. **Annual retrospective** on missed changes (if any) → root cause analysis and systemic fixes

The principle is that every metric in this framework exists not just to measure, but to drive action. A metric that is tracked but never acted upon is not a metric — it is theatre.
