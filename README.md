# RegRadar: AI-Powered Regulatory Change Management
### Product Portfolio Project | AI Product Management

---

## Overview

Regulatory change management is one of the most costly and error-prone operational challenges in financial services. UK banks collectively spend billions annually ensuring compliance with evolving FCA, PRA, and Basel requirements — yet the core process remains largely manual: compliance analysts read dense regulatory publications, manually cross-reference internal policies, and circulate impact assessments via email and spreadsheets.

**RegRadar** is a conceptual AI-powered product that transforms this process using Retrieval-Augmented Generation (RAG) to automatically ingest regulatory updates, map them to affected internal policies, and generate structured impact assessments for human review — reducing analyst time-to-triage by an estimated 70% while maintaining the audit trails required in regulated environments.

---

## Why This Problem

During over 5 years as a Product Manager at a major UK bank, I observed that regulatory change management was a persistent operational bottleneck. When the FCA issued a significant consultation paper or policy statement, a team of compliance analysts would spend days — sometimes weeks — manually reading the document, identifying which internal policies were affected, and writing impact summaries. This process was:

- **Slow**: A single regulatory update could take 2–3 weeks to fully triage
- **Inconsistent**: Output quality depended heavily on individual analyst experience
- **Poorly audited**: Impact assessments lived in email chains and shared drives
- **Reactive**: Teams often discovered relevant changes late in the implementation cycle

This is not a niche problem. Every UK-regulated bank faces this challenge, and the regulatory environment has only grown more complex post-Brexit.

---

## What This Project Contains

| File | Description |
|------|-------------|
| [`product-spec.md`](./product-spec.md) | Full PRD — problem, users, solution design, AI architecture, requirements, risks |
| [`research/competitive-landscape.md`](./research/competitive-landscape.md) | Market analysis of existing RegTech solutions and gaps |
| [`research/regulatory-volume-data.md`](./research/regulatory-volume-data.md) | Supporting data on the scale of the regulatory burden in UK banking |
| [`evaluation-framework.md`](./evaluation-framework.md) | How success would be measured across accuracy, efficiency, and adoption dimensions |

---

## Key Product Decisions & AI Design Principles

**Why RAG over a fine-tuned model?**
Regulatory documents change constantly. A RAG architecture allows the system to retrieve from an always-current document store rather than requiring retraining cycles — critical in an environment where a stale model could produce a dangerously outdated impact assessment.

**Why human-in-the-loop is non-negotiable?**
In a regulated industry, an AI-generated impact assessment cannot be the final word. The product is designed to *augment* analyst judgment, not replace it. Every output is clearly labelled as AI-generated, includes confidence indicators, cites its source documents, and requires a named human sign-off before any assessment is logged as reviewed.

**Why explainability matters more than accuracy alone?**
A 95% accurate model that cannot explain its reasoning is useless in a compliance context where regulators may audit decisions. RegRadar surfaces the specific clauses in both the regulatory document and the internal policy that drove each flagged impact — making every assessment fully traceable.

---

## Tech Stack (Conceptual)

- **Document Ingestion**: Automated scraping of FCA, PRA, and Bank of England publication feeds
- **Vector Store**: Pinecone or Weaviate for semantic search across internal policy library
- **LLM**: GPT-4o or Claude 3 Opus via API, with structured output prompting
- **RAG Orchestration**: LangChain or LlamaIndex
- **Human Review Interface**: Web app with approval workflows, audit logging, and comment threads
- **Audit Trail**: Immutable log of all AI outputs, human edits, and sign-offs

---

## About This Project

This is a portfolio project created to demonstrate AI product management thinking in a regulated financial services context. It is based on real industry problems observed during a career in UK banking, informed by publicly available regulatory data and RegTech market research.

**Author**: [Your Name]  
**Background**: 5+ years Product Management in UK Financial Services | PRINCE2 Agile Certified | AI PM (Coursera) | Perplexity AI Fellow  
**LinkedIn**: https://www.linkedin.com/in/loisanum/

