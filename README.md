# RAG System for Cybersecurity Practitioner Guidelines

A Retrieval-Augmented Generation system that generates phishing prevention guidelines for cybersecurity practitioners. Built for ARTI 6000 (Advanced Topics in AI/ML) at Adelaide University.

## What it does

Takes a practitioner's question about phishing (e.g. "How should we handle spear-phishing attempts in a mid-sized enterprise?"), retrieves relevant content from a curated knowledge base of authoritative cybersecurity sources, and generates 3 actionable guidelines grounded in the retrieved context.

## Architecture

- **Knowledge Base:** 197 documents from 76 source families (NIST, CISA, MITRE ATT&CK, OWASP, SANS, APWG, ISO 27001), including 84 scraped live from MITRE ATT&CK and NIST
- **Chunking:** 300 characters, 50 character overlap, 495 chunks
- **Embeddings:** all-MiniLM-L6-v2 (Microsoft, 384 dimensions)
- **Dense Retrieval:** FAISS index with inner product similarity
- **Sparse Retrieval:** BM25 keyword matching
- **Hybrid Retrieval:** Reciprocal Rank Fusion (k=60) combining dense and sparse
- **LLM:** Mistral-7B-Instruct-v0.2 with 4-bit NF4 quantisation (4.01 GB)
- **Evaluation:** Context relevance, answer relevance, faithfulness (RAGAS-style using MiniLM), BERTScore F1 (roberta-large)

## Results

Evaluated across 100 practitioner queries (200 total evaluations across dense and hybrid retrieval):

| Metric | Dense | Hybrid |
|---|---|---|
| Context Relevance | 0.679 | 0.645 |
| Answer Relevance | 0.688 | 0.680 |
| Faithfulness | 0.813 | 0.803 |
| BERTScore F1 | 0.873 | 0.876 |

Faithfulness (0.803) exceeds CyberBOT's 0.788 despite using a 10x smaller model. BERTScore (0.876) is within 6% of CyberBOT's 0.933.

## Dataset

- 5,873 total samples (495 chunks + 5,378 QA pairs)
- 120 unique topics
- 100 evaluation queries across 10 categories

## How to Run

1. Open the notebook in Google Colab
2. Set runtime to GPU (A100 recommended, T4 works but slower)
3. Run all cells from top to bottom
4. Total runtime: ~70 minutes on A100

## Requirements

- Google Colab Pro (or any environment with a GPU)
- HuggingFace account with access to Mistral-7B-Instruct-v0.2
- Python packages: sentence-transformers, transformers, bitsandbytes, faiss-gpu, rank-bm25, bert-score, pandas, matplotlib, seaborn

## Author

Joel Jorly (a1959854)
Adelaide University, May 2026
