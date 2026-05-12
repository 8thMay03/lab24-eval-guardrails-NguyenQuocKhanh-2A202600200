# Test Set Review Notes

## Review Process
Reviewed 12 out of 50 questions manually. Each question was checked for:
1. **Relevance** — Does it relate to the banking domain?
2. **Answerability** — Can it be answered from the document corpus?
3. **Clarity** — Is the question unambiguous?
4. **Ground truth accuracy** — Is the reference answer correct?

## Reviewed Questions

| # | Question | Type | Status | Notes |
|---|---|---|---|---|
| 1 | What is the minimum opening balance for a regular savings account? | simple | ✅ OK | Clear, single-fact answer |
| 2 | What is the interest rate for a regular savings account? | simple | ✅ OK | Directly from banking_products.md |
| 3 | What is the daily withdrawal limit at ATMs? | simple | ✅ OK | Single fact retrieval |
| 4 | What is the exact fee charged for withdrawing cash from another bank's ATM? | simple | ✏️ **EDITED** | Original was "How much is the interbank ATM withdrawal fee?" — edited for clarity and specificity |
| 5 | How long is the interest-free period for credit card payments? | simple | ✅ OK | |
| 6 | If a customer has 20M VND in savings, how much interest? | reasoning | ✅ OK | Requires calculation: 20M × 4.5% |
| 7 | Can a customer with 15M/month income qualify for 5M mortgage payment? | reasoning | ✅ OK | Multi-step: 15M ≥ 3×5M |
| 8 | Compare security measures for online vs ATM | multi_context | ⚠️ Broad | Acceptable but spans multiple sections |
| 9 | Lãi suất tiền gửi được điều chỉnh bao lâu một lần? | simple | ✅ OK | Vietnamese question, clear |
| 10 | Thời gian phản hồi khiếu nại ban đầu là bao lâu? | simple | ✅ OK | From policies document |
| 11 | What is the total cost for SWIFT transfer of 100M VND? | reasoning | ⚠️ Tricky | Min fee applies — ground truth correctly accounts for this |
| 12 | So sánh quy trình xử lý khiếu nại cấp 1 và cấp 3 | multi_context | ✅ OK | Cross-section comparison |

## Key Observations
1. **Distribution is correct**: 50% simple (25), 26% reasoning (13), 24% multi-context (12)
2. **Bilingual coverage**: ~20% questions in Vietnamese, matching the bilingual corpus
3. **Question #4 was manually edited** to improve clarity (requirement met)
4. **Reasoning questions** are well-designed — they require computation or multi-step inference
5. **Multi-context questions** properly span multiple document sections

## Issues Found
- Some multi-context questions may be too broad for extractive RAG → expected low scores
- Vietnamese questions may have lower retrieval quality due to BM25 tokenization

## Recommendation
Test set quality is **acceptable** for evaluation. The mix of EN/VN and simple/complex questions provides good coverage of the banking domain.
