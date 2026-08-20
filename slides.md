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
| **QnA Benchmark** (Exp 1) | <span class="status-progress">🔵 Generation pipeline run complete — questions with human reviewers (author / GSMA / Huawei), no completion estimate yet</span> |
| **Main Ingestion Grid** (Exp 2) | <span class="status-done">✅ Ingestion complete — 96/96 configurations</span> |
| **Retrieval Scoring** (Exp 2, cont.) | <span class="status-blocked">🔴 Not started — blocked on the QnA benchmark clearing human review</span> |
| **Domain Specialisation / Cost Analysis** (Exp 3) | <span class="status-blocked">🔴 Not started — blocked on retrieval scoring</span> |

* **QnA Benchmark:** the automated `telco-qna-generation` pipeline — sampling, generation, dedup, validation, competitor filter, single-metric judges, empirical difficulty jury — has run end-to-end. Surviving questions are in human review, the pipeline's final gate before merging to the canonical evaluation set.
* **Ingestion:** the full 96-configuration grid (chunking × enrichment × embedding model) is ingested and stored; nothing left pending here.
* **Retrieval scoring and Exp 3** both depend on the finalised, human-reviewed QnA set — the current critical path for the whole project.

---

# Recap: Motivation & Background

* LLMs are increasingly relied on across high-stakes domains; telecom is one where mistakes carry real operational consequences
* General-purpose embeddings show a **specificity gap** on telecom's acronyms, versioned specs, and formulaic language
* 3GPP specifications update cyclically — retrieval needs to keep pace, motivating Retrieval-Augmented Generation
* Grounded in Telco-RAG, Telco-oRAG, TSpec-LLM, MM-Telco
* **Core argument:** most existing optimisation work targets what happens *after* retrieval (reranking, prompting); the pre-embedding stage — chunking, enrichment, embedding model choice — is comparatively unexplored, despite determining what's available to retrieve in the first place

---

# Research Questions — Current Status

* **RQ1** — Effect of chunking strategy on retrieval accuracy and downstream generation? → pending retrieval scoring
* **RQ2** — Does chunk enrichment recover semantic information lost during chunking? → pending retrieval scoring
* **RQ3** — Does a domain-specific embedding model outperform general-purpose models, consistently across configurations? → pending retrieval scoring
* **RQ4** — What interaction effects exist between chunking, enrichment, and embedding model, and can they inform practical guidance? → pending retrieval scoring
* **RQ5** — Can a strong pre-embedding configuration let a smaller, cheaper generation model match or beat a larger one, at lower cost? → not yet started; depends on RQ1–4 identifying a strong configuration to test against

*All five RQs are now blocked on the same thing: the QnA benchmark clearing human review, which unblocks retrieval scoring, which unblocks everything downstream.*

---

# Research Methodology & Structure

* **QnA Benchmark:** 🔵 generation run complete, human review in progress
  * `telco-qna-generation` toolkit ran end-to-end against 3GPP Rel-19; surviving questions split three ways for expert review
* **Main Ingestion Grid:** ✅ complete
  * 96 configurations (embedding model × chunking × enrichment) ingested via `master_grid.py` + `llm_ingestion_grid.py`
* **Retrieval Scoring:** 🔴 not started — addresses RQ1–RQ4, blocked on the finalised QnA set
* **RQ5 Comparison:** 🔴 not started — a smaller, targeted follow-up once a strong configuration is identified from RQ1–RQ4

---

# QnA Benchmark — Scope & Tooling

**Tool:** `telco-qna-generation` — standalone, installs into TelcoLens for GUI review

**Corpora supported:** 3GPP, O-RAN, ETSI, ITU-T, GSMA, CAMARA, TM Forum
**Current focus:** 3GPP Rel-19

---

# QnA Benchmark — Generation Pipeline (Blocks)

1. Sample chunks (stratified / section-boundary) <span class="status-done">✅</span>
2. Generate questions — concurrent LLM calls, mixed formats (open/tf/mc) <span class="status-done">✅</span>
3. Dedup — remove near-identical questions <span class="status-done">✅</span>
4. Validate Answerable — faithfulness pass <span class="status-done">✅</span>
5. Competitor Filter — drop questions answerable from a competing chunk <span class="status-done">✅</span>
6. Single-metric LLM judges — Faithfulness, Relevance, Semantic Correctness, Telecom Domain Relevance <span class="status-done">✅</span>
7. Empirical Difficulty Jury — drops "too easy" and "unanswerable" questions <span class="status-done">✅</span>
8. Human expert review (Approved / Corrected / Rejected) before merging to canonical eval set <span class="status-progress">🔵 in progress</span>

---

# QnA Benchmark — Human Review

* Reviewers: author, GSMA, Huawei — split evenly across the surviving question set
* Each question reviewed for correctness and appropriate difficulty; reviewers may approve, correct, or reject
* Review is in progress — no completion estimate yet
* Once complete, approved/corrected questions merge into the canonical evaluation set used for retrieval scoring

---

# Main Ingestion Grid — Complete

**Tool:** `master_grid.py` (non-LLM strategies) + `llm_ingestion_grid.py` (LumberChunker, LLM-generated metadata) — both push completed collections to Hugging Face (`LiamDuero/telcolens-chunks`)

| Dimension | Options | Count |
| :-- | :-- | :-: |
| Embedding models | minilm, mpnet, e5, bge, otel-109m, otel-0.6b | 6 |
| Chunking strategies | text_baseline, sliding_window_tokens, parent_child, hierarchical_markdown, lumber_chunker | 5 |
| Enrichment | none, metadata_tagging, acronym_expansion, llm_metadata* | up to 4 |
| **Total configurations** | | **96 — ✅ complete** |

*\*llm_metadata is only run against lumber_chunker, not all five chunking strategies — the enrichment sweep is not fully symmetric across strategies.*

---

# Retrieval Scoring — Not Yet Started

* Blocked on the QnA benchmark clearing human review
* Once unblocked: score all 96 ingested configurations against the finalised question set (retrieval accuracy, answer correctness, efficiency)
* Directly addresses RQ1–RQ4
* Retrieval mode (dense/BM25/hybrid) and reranking are held fixed for this main pass — varied separately if time allows

---

# RQ5 — Cost/Performance Comparison — Not Yet Started

**Question:** does a strong pre-embedding configuration let a smaller, cheaper generation model match or beat a larger model paired with a weak configuration?

**Planned approach:**
1. Identify the best- and worst-performing configurations from the main grid (once scored)
2. Re-run both against a small and a large generation model
3. Compare overall performance and cost between {best config, small model} and {worst config, large model}

*Depends entirely on retrieval scoring completing first.*

---

# Contributions — Expected

* An open-source, model-agnostic RAG pipeline for telecom document ingestion and retrieval
* A 96-configuration empirical study of chunking, enrichment, and embedding model choice (RQ1–RQ4)
* A validated, expert-reviewed 3GPP QnA benchmark, produced via a novel automated quality-control pipeline
* A practitioner-facing recommendation matrix for pre-embedding configuration choices
* If RQ5 holds: evidence that pre-embedding quality can substitute for generation-model scale, at lower cost
* Open-source release of both the pipeline and the benchmark; the benchmark forms the evaluation basis for a GSMA-linked competition

---

# Next Steps

* Complete human review of the generated question set (author / GSMA / Huawei)
* Merge approved/corrected questions into the canonical evaluation set
* Run retrieval scoring across all 96 ingested configurations (RQ1–RQ4)
* Identify best/worst configurations and run the RQ5 cost comparison
* Synthesise results into the recommendation matrix
