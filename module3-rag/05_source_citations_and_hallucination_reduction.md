# Source Citations + Hallucination Reduction

## The Problem
LLMs can make things up even with RAG. Critical for medical/legal/financial data.

## Strategy 1: Cite Sources in Prompt
```
"Answer ONLY from provided records. Cite source in [brackets].
If not found, say 'Not found in records.' Never guess."
```

## Strategy 2: Return Sources with Response
```python
response = query_engine.query("What medications?")
print(response.response)                    # answer
print(response.source_nodes[0].text)        # chunk used
print(response.source_nodes[0].metadata)    # file, page number
```

## Strategy 3: Constrain the LLM
```
✗ "What does this lab result mean?" → might give medical advice
✓ "What does the record say about X?" → reports data only
```

## Hallucination Checklist
1. Temperature 0.0 — deterministic
2. "Only answer from context" in system prompt
3. "Say I don't know" fallback
4. Return source chunks for verification
5. Reranking — better retrieval = less hallucination
