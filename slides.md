---
marp: true
theme: default
paginate: true
style: |
  section {
    font-size: 25px;
  }
  h1 {
    font-size: 33px;
  }
  .status-done { color: #1a7f37; font-weight: bold; }
  .status-progress { color: #9a6700; font-weight: bold; }
  .status-blocked { color: #cf222e; font-weight: bold; }
---

# Empirical Evaluation of Domain-Specific and General-Purpose Embedding Models in Telecom
## Progress Update

**Liam Duero**

* Industry Supervisor: Antonio de Domenico
* Academic Supervisor: Jagmohan Chauhan

![height:160px](img/Granular%20Research%20Proposal%20V2_0.png)

---

# Progress at a Glance

| Workstream | Status |
| :-- | :-- |
| **Exp 1** — Ground Truth QA Set | <span class="status-progress">🔵 Full generation run complete — questions with human reviewers (author / GSMA / Huawei) for final approval</span> |
| **Exp 2** — Comparative Retrieval Benchmarking | <span class="status-progress">🔵 Ingestion grid running — 96-configuration grid, in progress</span> |
| **Exp 3** — Domain Specialisation Necessity | <span class="status-blocked">🔴 Not started — blocked on Exp 1 (final set) and Exp 2 (scoring)</span> |

* **Exp 1:** the automated `telco-qna-generation` pipeline — sampling, generation, dedup, validation, competitor filter, single-metric judges, empirical difficulty jury — has now run end-to-end. Surviving questions are in human review, the pipeline's final gate before merging to the canonical evaluation set.
* **Exp 2:** ingestion grid actively running; retrieval scoring against ground truth still blocked on Exp 1's finalised set.
* **Exp 3:** blocked on the three open questions — untouched.

---

# Recap: Motivation & Background

* AI-native infrastructure for 6G pushes toward domain-specific embeddings, partly to cut GPU cost
* General-purpose embeddings have a **specificity gap** on telecom's acronyms, versioned specs, formulaic language
* Standards evolve fast — retrieval needs to keep pace
* Grounded in Telco-RAG, Telco-oRAG, TSpec-LLM, MM-Telco

---

# Research Objectives — Current Status

* **[Q1]** Domain-specific vs. general-purpose embeddings? → pending Exp 2 retrieval scoring
* **[Q2]** How do chunking + enrichment interact? → pending Exp 2 retrieval scoring
* **[Q3]** Auto-generate grounded test sets → 🔵 full pipeline run complete, questions in human review
* **[Q4]** Which document traits require specialised embeddings? → not started (Exp 3)
* **[Q5]** Contribute to open telco AI initiative → 🔵 ongoing — `telco-qna-generation` and ingestion grid both feed the GSMA Open Telco AI Initiative

---

# Research Methodology & Structure

* **Exp 1 — Ground Truth QA set:** 🔵 generation run complete, human review in progress
  * `telco-qna-generation` toolkit ran end-to-end against 3GPP Rel-19; surviving questions split three ways for expert review
* **Exp 2 — Comparative Retrieval Benchmarking:** 🔵 ingestion grid running
  * 96-configuration grid (embedding model × chunking × enrichment) via `master_grid.py` + `llm_ingestion_grid.py`
* **Exp 3 — Domain Specialisation Necessity:** 🔴 not started
  * Depends on both Exp 1 (final QA set) and Exp 2 (scoring results)

---

# Exp 1: Ground Truth QA Set — Scope & Tooling

**Tool:** `telco-qna-generation` — standalone, installs into TelcoLens for GUI review

**Corpora supported:** 3GPP, O-RAN, ETSI, ITU-T, GSMA, CAMARA, TM Forum
**Current focus:** 3GPP Rel-19

---

# Exp 1: Generation Pipeline (Blocks) — Complete Through Stage 7

1. Sample chunks (stratified / section-boundary) <span class="status-done">✅</span>
2. Generate questions — concurrent LLM calls, mixed formats (open/tf/mc/fill-blank) <span class="status-done">✅</span>
3. Dedup — remove near-identical questions <span class="status-done">✅</span>
4. Validate Answerable — faithfulness pass <span class="status-done">✅</span>
5. Competitor Filter — drop questions answerable by ≥2 chunks (ambiguous) <span class="status-done">✅</span>
6. Single-metric LLM judges — Grounding, Faithfulness, Domain Relevance, Semantic Correctness, Relevance <span class="status-done">✅</span>
7. Empirical Difficulty Jury — drops "too easy" and "unanswerable" questions <span class="status-done">✅</span>
8. Human expert review (Approved / Corrected / Rejected) before merging to canonical eval set <span class="status-progress">🔵 in progress</span>

*Stages 1–7 have now run end-to-end on real data.*

---

# Exp 1: Human Review — Current Status

* Reviewers: author, GSMA, Huawei — split evenly across the surviving question set
* Each question reviewed for correctness and appropriate difficulty; reviewers may approve, correct, or reject
* Review is in progress — no completion estimate yet
* Once complete, approved/corrected questions merge into the canonical evaluation set used for Exp 2 retrieval scoring

---

# Exp 2: Comparative Retrieval Benchmarking — the Grid

**Tool:** `master_grid.py` — non-LLM ingestion grid orchestrator, pushes each collection to Hugging Face (`LiamDuero/telcolens-chunks`)

| Dimension | Options | Count |
| :-- | :-- | :-: |
| Embedding models | minilm, mpnet, e5, bge, otel-109m, otel-0.6b | 6 |
| Chunking strategies | text_baseline, sliding_window_tokens, parent_child, hierarchical_markdown, lumber_chunker | 5 |
| Enrichment | none, metadata_tagging, acronym_expansion, llm_metadata | up to 4 |
| **Total configurations** | | **96** |

*Non-LLM strategies run via `master_grid.py`; `lumber_chunker` (and `llm_metadata` enrichment) run via `llm_ingestion_grid.py`, since chunking there requires an LLM call.*

---

# Exp 2: Ingestion Progress

* Ingestion in progress across the 96-configuration grid — resumes from the next pending run onward
* Each completed run's vector store is pushed to Hugging Face under `ingestion-grid/{run_name}`
* Distance metric: cosine; batch size 4096 for GPU throughput
* Grid halts after 2 consecutive failures in a model family, to avoid burning through a broken config set
* **Not yet started:** retrieval scoring against ground-truth QA (blocked on Exp 1's human review completing)

---

# Exp 3: Planned Approach (once Exp 1 & 2 complete)

**Models:** winning configuration from Exp 2, per sub-domain
**Data:** Exp 2's retrieval results and error logs
**Experiment:** determine where/why specialised embeddings beat or lose to general-purpose ones
**Planned results:** verdict on which telecom areas need specialised embeddings, plus a decision matrix by sub-domain

---

# Exp 3: Planned Workflow

1. Extract errors — isolate queries where the specialised model significantly under/over-performs
2. Categorise root causes — retrieval ambiguity, version sensitivity, formulaic/numerical queries
3. Re-test the winning configuration to confirm findings hold

---

# Contributions to Science (Academic) — Expected

* Empirical evaluation of domain-specific embeddings
* Automated benchmarking frameworks and RAG pipelines for 3GPP
* Evaluation of retrieval granularity for technical standards
* Quantifying the performance gap: general-purpose vs. domain-specific models

---

# Impact Statement (Business / Industry) — Expected

* **Operational Efficiency & Cost Reduction:** specialised embeddings hitting higher accuracy with smaller models
* **Risk Mitigation:** evidence-based model/pipeline recommendations grounded in technical standards, reducing hallucination risk

---

# Next Steps

* Complete human review of the generated question set (author / GSMA / Huawei)
* Merge approved/corrected questions into the canonical evaluation set
* Complete the remaining ingestion grid configurations
* Run Exp 2 retrieval scoring once both the finalised QA set and ingestion grid are ready
* Resolve the three Exp 3 open questions before starting that analysis
