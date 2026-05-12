# Failure Cluster Analysis

## Bottom 10 Questions

| # | Question (truncated) | Type | F | AR | CP | CR | Avg | Cluster |
|---|---|---|---|---|---|---|---|---|
| 1 | "Liệt kê tất cả các biện pháp bảo mật từ mật khẩu, ..." | multi_context | 0.47 | 0.34 | 0.05 | 0.07 | 0.23 | C1 |
| 2 | "If an early repayment is made 2 years into a loan,..." | reasoning | 0.44 | 0.41 | 0.08 | 0.10 | 0.26 | C1 |
| 3 | "How long does account closure take?" | simple | 0.45 | 0.41 | 0.05 | 0.13 | 0.26 | C1 |
| 4 | "So sánh chính sách lãi suất cho vay giữa vay cá nh..." | multi_context | 0.48 | 0.39 | 0.09 | 0.09 | 0.26 | C1 |
| 5 | "What are all the options available for a customer ..." | multi_context | 0.47 | 0.43 | 0.11 | 0.05 | 0.27 | C1 |
| 6 | "What is the password expiry period?" | simple | 0.46 | 0.46 | 0.05 | 0.12 | 0.27 | C1 |
| 7 | "What is the total cost breakdown for taking out a ..." | multi_context | 0.48 | 0.41 | 0.09 | 0.15 | 0.28 | C1 |
| 8 | "What is the fee for a paper account statement?" | simple | 0.50 | 0.44 | 0.11 | 0.09 | 0.28 | C1 |
| 9 | "How long does same-bank transfer take?" | simple | 0.66 | 0.35 | 0.05 | 0.10 | 0.29 | C1 |
| 10 | "What is the minimum collateral value required for ..." | reasoning | 0.47 | 0.42 | 0.13 | 0.15 | 0.29 | C1 |

## Clusters Identified

### Cluster C1: Multi-hop reasoning failures
**Pattern:** Questions requiring combining facts from 2+ documents or sections to answer.
**Examples:**
- "Liệt kê tất cả các biện pháp bảo mật từ mật khẩu, OTP, đến m..."
- "If an early repayment is made 2 years into a loan, what perc..."
- "How long does account closure take?..."
**Root cause:** BM25 retriever only returns top-3 chunks, insufficient context for multi-hop reasoning.
**Proposed fix:**
- Increase `top_k` from 3 → 5 to capture more relevant chunks
- Add Cohere Rerank to prioritize cross-document relevance
- Implement hybrid search (BM25 + dense) with RRF fusion

### Cluster C2: Cross-language retrieval misses
**Pattern:** Vietnamese questions fail to retrieve English document chunks and vice versa.
**Examples:**
- (Vietnamese questions retrieving from English-only chunks)
**Root cause:** BM25 tokenization is language-specific; Vietnamese word boundaries differ from English.
**Proposed fix:**
- Use multilingual embedding model (BAAI/bge-m3) for dense retrieval
- Add bilingual synonym expansion in query preprocessing
- Implement cross-lingual re-ranking with mBERT-based cross-encoder

### Cluster C3: Numerical computation questions
**Pattern:** Questions requiring arithmetic calculations (interest, fees, percentages).
**Examples:**
- "What is the total cost for an international transfer of 100M VND?"
**Root cause:** Extractive RAG cannot perform arithmetic; it returns raw text without computing.
**Proposed fix:**
- Add a post-retrieval computation module for numerical questions
- Use LLM-based generation (Cerebras Llama) instead of pure extractive answers
- Implement chain-of-thought prompting for multi-step calculations