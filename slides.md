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
| **Exp 1** — Ground Truth QA Set | <span class="status-progress">🔵 Pipeline built; generation logic + judges being validated; full run not yet executed</span> |
| **Exp 2** — Comparative Retrieval Benchmarking | <span class="status-progress">🔵 Ingestion grid running — 60/84 configs complete</span> |
| **Exp 3** — Domain Specialisation Necessity | <span class="status-blocked">🔴 Not started — no findings yet</span> |

* **Exp 1:** the `telco-qna-generation` pipeline is built; still finalising the flow/structure before running it fully — the numbers on Exp 1's slides are projections under consideration, not results
* **Exp 2:** the non-LLM ingestion grid (`master_grid.py`) is actively running; retrieval scoring against ground truth hasn't started
* **Exp 3:** blocked on the three open questions — untouched

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
* **[Q3]** Auto-generate grounded test sets → 🔵 pipeline built, structure being finalised, no full run yet
* **[Q4]** Which document traits require specialised embeddings? → not started (Exp 3)
* **[Q5]** Contribute to open telco AI initiative → 🔵 ongoing — `telco-qna-generation` and ingestion grid both feed the GSMA Open Telco AI Initiative

---

# Research Methodology & Structure

* **Exp 1 — Ground Truth QA set:** 🔵 pipeline built, being validated
  * `telco-qna-generation` toolkit generating draft QnA against 3GPP Rel-19
* **Exp 2 — Comparative Retrieval Benchmarking:** 🔵 ingestion grid running
  * 84-configuration grid (embedding model × chunking × enrichment) via `master_grid.py`
* **Exp 3 — Domain Specialisation Necessity:** 🔴 not started
  * Depends on both Exp 1 (final QA set) and Exp 2 (scoring results)

---

# Exp 1: Ground Truth QA Set — Scope & Tooling

**Tool:** `telco-qna-generation` — standalone, installs into TelcoLens for GUI review

**Corpora supported:** 3GPP, O-RAN, ETSI, ITU-T, GSMA, CAMARA, TM Forum
**Current focus:** 3GPP Rel-19

---

# Exp 1: Generation Pipeline (Blocks)

1. Sample chunks (stratified / section-boundary)
2. Generate questions — concurrent LLM calls, mixed formats (open/tf/mc/fill-blank)
3. Dedup — remove near-identical questions
4. Validate Answerable — faithfulness pass
5. Competitor Filter — drop questions answerable by ≥2 chunks (ambiguous)
6. Single-metric LLM judges — Grounding, Faithfulness, Domain Relevance, Semantic Correctness, Relevance
7. Empirical Difficulty Jury — drops "too easy" and "unanswerable" questions
8. Human expert review (Approved / Corrected / Rejected) before merging to canonical eval set

*This flow and its judge blocks are still being validated — not yet run end-to-end.*

---

# Exp 1: Projected Funnel (under consideration, not yet run)

| Stage | Count |
| :-- | :-- |
| Chunks sampled (stratified, RAN1-4/SA1-6/CT1/3/4) | ~600–700 |
| Raw questions generated | ~1,800–2,000 |
| After Dedup | ~1,600 |
| After Validate Answerable | ~1,300 |
| After Competitor Filter | ~1,150 |
| After single-metric judges | ~1,000–1,050 |
| After Empirical Difficulty Jury | **~950–1,000 (projected final)** |

*These are planning estimates while the flow structure is finalised — not results.*

---

# Exp 2: Comparative Retrieval Benchmarking — the Grid

**Tool:** `master_grid.py` — non-LLM ingestion grid orchestrator, pushes each collection to Hugging Face (`LiamDuero/telcolens-chunks`)

| Dimension | Options | Count |
| :-- | :-- | :-: |
| Embedding models | minilm, mpnet, e5, bge, otel-109m, otel-300m, otel-0.6b | 7 |
| Chunking strategies | text_baseline, sliding_window_tokens, parent_child, hierarchical_markdown | 4 |
| Enrichment | none, metadata_tagging, acronym_expansion | 3 |
| **Total configurations** | | **84** |

---

# Exp 2: Ingestion Progress

* **60 / 84 configurations complete** (~71%) — ingestion resumes from run 61 onward
* Each completed run's vector store is pushed to Hugging Face under `ingestion-grid/{run_name}`
* Distance metric: cosine; batch size 4096 for GPU throughput
* Grid halts after 2 consecutive failures in a model family, to avoid burning through a broken config set
* **Not yet started:** retrieval scoring against ground-truth QA (depends on Exp 1's finalised set)

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

* Finish validating Exp 1's generation flow and judge blocks; finalise structure
* Run the full QnA generation pass once structure is finalised
* Complete the remaining 24 ingestion grid configurations
* Run Exp 2 retrieval scoring once both the QA set and ingestion grid are ready
* Resolve the three Exp 3 open questions before starting that analysis