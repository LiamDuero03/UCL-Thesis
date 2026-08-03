<span style="color:#242424">Jagmohan Chauhan</span>

![](img%5CGranular%20Research%20Proposal%20V2_0.png)

# Empirical Evaluation of Domain-Specific and General-Purpose Embedding Models in Telecom

Liam Duero

Industry Supervisor: Antonio de DomenicoAcademic Supervisor: Jagmahon Chauhan

![](img%5CGranular%20Research%20Proposal%20V2_1.png)

# Research Motivations

Complexity of Telecom Data and resulting need for AI\-native infrastructure

Specificity Gap in General\-purpose LLMs

Rapidly Evolving technical standards and need to keep up with this pace

![](img%5CGranular%20Research%20Proposal%20V2_2.png)

# Industry Background and Publications



* Industry background
  * 6G visions require AI\-native infrastructure where LLMs and specialised RAG systems automate large workload
  * Shift towards Small Language Models and domain\-specific embeddings to reduce GPU costs
* Key publications
  * Telco\-RAG: Navigating the Challenges of Retrieval\-Augmented Language Models for Telecommunications
  * Telco\-oRAG: Optimizing Retrieval\-augmented Generation for Telecom Queries via Hybrid Retrieval and Neural Routing
  * TSpec\-LLM: An Open\-source Dataset for LLM Understanding of 3GPP Specifications
  * MM\-Telco: Benchmarks and Multimodal Large Language Models for Telecom Applications


![](img%5CGranular%20Research%20Proposal%20V2_3.png)

# Research Objectives

\[Q1\] Determine if a domain\-specific embedding model \(e\.g\.\, Bell\-Embedding\) is required to achieve technical accuracy in telecom retrieval tasks\, or if general\-purpose models \(e\.g\.\, OpenAI\) are sufficient

\[Q2\] Quantify how retrieval effectiveness is influenced by the interaction between document structure \(chunking strategy\) and enrichment in the telecom domain

\[Q3\] Develop a methodology to automatically generate high\-fidelity\, grounded test sets from 3GPP documentation

\[Q4\] Identify the specific document characteristics \(e\.g\.\, acronym density\, versioning\, formulaic complexity\) that dictate the necessity of specialized embeddings over general\-purpose alternatives\.

\[Q5\] Meaningfully contribute to the open telco AI initiative and their open platform initiative\, by contributing to the engineering of a repeatable RAG pipeline and testing suite

![](img%5CGranular%20Research%20Proposal%20V2_4.png)

# Research Methodology & Structure



* __Exp__  __ 1__  __ \- __  __Ground Truth model\-agnostic QA set__  __:__
  * Create a high\-fidelity telecom benchmark from 3GPP \(start with Rel\-19\) documents for specific comparison requirements
* __Exp__  __ 2 \- __  __Comparative Retrieval Benchmarking \(parameterise the RAG pipeline\)__  __:__
  * Compare Chunking Strategies \(Structured\, Semantic\)
  * Compare different Enrichment Strategies \(Glossary\, Metadata\)
  * Compare different embedding models
* __Exp__  __ 3 __  __Domain Specialisation Necessity__  __:__
  * Analyse which subdomains require specialised embeddings the most
  * Expand Comparative benchmark with efficiency metrics


![](img%5CGranular%20Research%20Proposal%20V2_5.png)

# Exp 1: Ground Truth QA set

__Experiment__

Create a technical QA dataset from unstructured standards

Automate “Judge” validation to filter for technical accuracy and domain relevance

Expand existing GSMA pipeline

__Models__

Small model for generation \(e\.g\.\, gpt\-4o\-mini\)

Large model for validation \(e\.g\.\, gpt\-oss\-120b\)

__Data__

TSpec\-LLM dataset

3GPP Rel 19

__Results__

A validated JSON File containing technical question and answers

![](img%5CGranular%20Research%20Proposal%20V2_6.png)

# Exp 1: Synthetic Dataset Generation & Technical Validation

![](img%5CGranular%20Research%20Proposal%20V2_7.png)

# Exp 1: Workflow

![](img%5CGranular%20Research%20Proposal%20V2_8.png)

![](img%5CGranular%20Research%20Proposal%20V2_9.png)

![](img%5CGranular%20Research%20Proposal%20V2_10.png)

# Exp 1: Judge Validation Logic

![](img%5CGranular%20Research%20Proposal%20V2_11.png)

# Exp 2: Comparative Retrieval Benchmarking



* __Experiment__
* Comparison of following independent variables:
  * Chunking Strategies
  * Enrichment Strategies
  * Embedding models
  * Parameterise any aspect of the RAG pipeline


__Models__

AT&Ts Bell Embedding\-xxx

Open AI models

__Data__

Validated QA JSON File from Exp 1

TspecLLM

__Results__

Performance metric table centered around NDCG@10 across different sub\-domains

Consider Efficiency Metrics

![](img%5CGranular%20Research%20Proposal%20V2_12.png)

|  | Structured Chunking \(Fixed Token Size\) | Semantic Chunking |
| :-: | :-: | :-: |
| No Enrichment | Test 1 | Test 2 |
| Glossary Enrichment | Test 3 | Test 4 |
| Metadata Enrichment | Test 5 | Test 6 |
| Glossary \+ Metadata Enrichement | Test 7 | Test 8 |

This table will be expanded based on capacity to test different configurations

Run this for different Models:

Bell\-embedding\-xxx

text\-embedding\-3\-large

![](img%5CGranular%20Research%20Proposal%20V2_13.png)

# Exp 2: Workflow

Models:

Bell\-embedding\-xxx

text\-embedding\-3\-large

![](img%5CGranular%20Research%20Proposal%20V2_14.png)

![](img%5CGranular%20Research%20Proposal%20V2_15.png)

![](img%5CGranular%20Research%20Proposal%20V2_16.png)

![](img%5CGranular%20Research%20Proposal%20V2_17.png)

![](img%5CGranular%20Research%20Proposal%20V2_18.png)

# Exp 2: Evaluation Metrics & Scoring

Granular Analysis: Categorise retrieval performance by  __3GPP sub\-domains__  \(RAN\, Core Network\, Security …\) to identify specific niches where specialised embeddings outperform general models\.

Current Evaluation Suite contains 5 metrics\, to be expanded

![](img%5CGranular%20Research%20Proposal%20V2_19.png)

# Exp 3: Domain Specialisation Necessity

__Models__

The winning model configuration from Exp 2 for each subdomain

__Data__

The retrieval results and error logs from Exp 2

__Experiment__

Determining where and why the specialized model succeeds or fails compared to the general one

__Results__

Final verdict on what areas in telecom require specialised embeddings

Decision Matrix mapping approach to subdomain

![](img%5CGranular%20Research%20Proposal%20V2_20.png)

# Exp 3: Workflow

![](img%5CGranular%20Research%20Proposal%20V2_21.png)

# Exp 3: Assessment Approach

![](img%5CGranular%20Research%20Proposal%20V2_22.png)

# Exp 3: Current Blockers/Questions

![](img%5CGranular%20Research%20Proposal%20V2_23.png)

# Exp 3: Takeaway

![](img%5CGranular%20Research%20Proposal%20V2_24.png)

# Contributions to Science (academic)

Empirical Evaluation of Domain\-specific embeddings

Advancement of Automated Benchmarking Frameworks and RAG pipelines for 3GPP

Evaluation of Retrieval Granularity for Technical Standards

Quantifying the performance gap between general purpose and domain\-specific models

![](img%5CGranular%20Research%20Proposal%20V2_25.png)

# Impact Statement (business/industry)



* Operational Efficiency and Cost Reduction
  * Proving that specialised embeddings can achieve higher accuracy with smaller models
* Providing an evidence\-based recommendation for model choices and pipelines for decisions grounded in technical standards\, mitigating business risk of hallucinated responses


![](img%5CGranular%20Research%20Proposal%20V2_26.png)

# Next Steps

Design Ground Truth QA creation pipeline

Design Semantic Chunking Strategy

Design Glossary Enrichment Strategy

Design Metadata Enrichment Strategy

