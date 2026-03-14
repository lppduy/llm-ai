# Evaluation — Measuring LLM App Quality

## What to Measure

### RAG Apps
| Metric | Measures |
|--------|---------|
| Retrieval relevance | Right chunks fetched? |
| Faithfulness | Answer matches context? |
| Answer correctness | Actually right? |
| Hallucination rate | Made things up? |

### General LLM Apps
Helpfulness, harmlessness, format compliance, latency, cost per query.

## Evaluation Methods

### 1. Human Eval (Gold Standard)
Rate responses 1-5. Accurate but slow, expensive, doesn't scale.

### 2. LLM-as-Judge (Scalable)
```python
eval_prompt = """Rate 1-5 for accuracy and completeness.
Question: {question}
Context: {context}
Answer: {answer}
Score (1-5):
Reasoning:"""

eval = client.chat.completions.create(
    model="gpt-4o",  # strong model as judge
    messages=[{"role": "user", "content": eval_prompt}]
)
```

### 3. Automated Metrics
```python
# Exact match
correct = predicted.strip().lower() == expected.strip().lower()

# Keyword check
found = all(kw in response for kw in expected_keywords)

# Embedding similarity
score = cosine_similarity(embed(response), embed(expected))
```

## Building an Eval Set
```python
eval_set = [
    {"question": "What medications?",
     "expected": "Metformin 500mg, Lisinopril 10mg",
     "context_should_contain": "medication list"},
    # ... 20-100 examples
]
```
Cover: happy path, edge cases, out-of-scope ("I don't know").

## Running Evals
```python
results = []
for item in eval_set:
    response = rag_pipeline(item["question"])
    retrieval_hit = item["context_should_contain"] in str(response.sources)
    score = llm_judge(item["question"], response.answer, item["expected"])
    results.append({"retrieval_hit": retrieval_hit, "score": score})

avg = sum(r["score"] for r in results) / len(results)
retrieval = sum(r["retrieval_hit"] for r in results) / len(results)
```

## Frameworks
| Tool | Focus | Cost |
|------|-------|------|
| RAGAS | RAG metrics | Free |
| DeepEval | General LLM eval | Free tier |
| Langfuse | Traces + evals | Free self-host |
| LangSmith | LangChain eval | Free tier |

## When to Eval
- Every prompt change → run eval set
- Before deploy → full eval, compare baseline
- After deploy → sample production queries weekly
- After model upgrade → re-run full eval

## Best Approach
Automated first → LLM-as-judge for edge cases → human spot-check.

## Exercise
Create 10-question eval set for med records chatbot. Score manually 1-5. Calculate average.
