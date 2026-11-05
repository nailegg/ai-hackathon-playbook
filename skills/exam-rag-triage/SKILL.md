---
name: exam-rag-triage
description: Time-boxed retrieval-augmented QA design for 180-minute tasks, focusing on grounded citations, version conflicts, abstention and small evaluations.
---

# Exam RAG Triage

Read documents' schema, versions, access rules and sample questions. Produce **at most one page**:
1. Minimal retrieval baseline that works without external infrastructure
2. Citation format that points to actual source identifiers/sections
3. Outdated/conflicting document policy and abstention cases
4. Retrieval, answer, citation, and refusal test cases
5. API/LLM unavailable fallback and explicit quality limitations

Do **not** modify code or source documents. Prefer testable grounded retrieval over agent-framework complexity. Return a specification the Main Codex can implement.
