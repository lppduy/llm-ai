# Hybrid Search + Reranking

## Problem with Semantic-Only
Good for meaning, bad for exact matches (dates, drug names, lab values).

## Hybrid Search = Semantic + Keyword
```
1. Semantic search → top 10 by meaning
2. Keyword search (BM25) → top 10 by exact words
3. Merge + combine scores → return top K
```

## Reranking
After retrieval, re-score results with a more powerful model:
```
1. Retrieve top 20 (fast, rough)
2. Reranker scores each against query (slow, precise)
3. Return top 5 (best results first)
```

## When to Use
- Basic RAG → semantic only (start here)
- Production → hybrid (handles exact terms)
- High-stakes (medical, legal) → hybrid + reranking
