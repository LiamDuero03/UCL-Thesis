# Project Status & Progress Log

## Current Status (As of 13/08/2026)
* **Data Ingestion:** Successfully configured the deterministic non-LLM ingestion grid and also the LLM aided chunking strategy. First Pass with 90 Configurations of embedding models, chunking strategies and enrichments done.
* **Infrastructure:** Transitioned pipeline to be functional on GPU, local PC and using openrouter
* **Storage:** Configured automated database backups directly to Hugging Face Hub ([`LiamDuero03/telcolens-chunks`](https://huggingface.co/datasets/LiamDuero/telcolens-chunks)).

## Currently In-Progress
* Validating the QnA Pipeline and checking first generated questions for usefulness
* Validating evaluaiton metrics on sample of evaluated results

## Next Steps (Next Week)
* Create the final QNA question set with reviewed questions
* Explore ingesting the original documents

