# Observability (Langfuse / LangSmith)

## What Is It?
Logging/tracing every LLM call: what sent, what returned, latency, cost, errors. Like APM but for LLM apps.

## What to Track Per Request
Input, output, model, token count, latency, cost, retrieved chunks, tool calls, user/session ID, errors.

## Tool Comparison
| | Langfuse | LangSmith |
|--|---------|-----------|
| Open source | Yes | No |
| Framework agnostic | Yes | LangChain-focused |
| Best for | Any LLM app | LangChain apps |
| Cost | Free self-host | Free tier |

**Side projects → Langfuse (free, works with anything).**

## Langfuse Quick Start
```bash
pip install langfuse
```

```python
from langfuse.decorators import observe

@observe()
def rag_pipeline(question: str):
    chunks = retrieve(question)
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {"role": "system", "content": f"Context: {chunks}"},
            {"role": "user", "content": question}
        ]
    )
    return response.choices[0].message.content
```
`@observe()` auto-logs: input, output, latency, model, tokens.

## Manual Tracing
```python
trace = langfuse.trace(name="rag-query", user_id="user-123")
span = trace.span(name="retrieval", input={"query": question})
chunks = retrieve(question)
span.end(output={"chunks": len(chunks)})

generation = trace.generation(
    name="llm-call", model="gpt-4o-mini",
    input=messages, output=response_text,
    usage={"input": prompt_tokens, "output": completion_tokens}
)
```

## DIY Observability (No External Tool)
```python
import json, time, logging
logger = logging.getLogger("llm")

def traced_llm_call(messages, model="gpt-4o-mini"):
    start = time.time()
    response = client.chat.completions.create(model=model, messages=messages)
    logger.info(json.dumps({
        "model": model,
        "input_tokens": response.usage.prompt_tokens,
        "output_tokens": response.usage.completion_tokens,
        "latency_ms": int((time.time() - start) * 1000),
    }))
    return response
```

## What to Monitor
1. Latency distribution (p50, p95, p99)
2. Cost per day/week
3. Error rate
4. Token usage trends
5. Low-score queries from eval

## When to Use What
```
Early stage     → DIY logging
Need debugging  → Langfuse cloud (free tier)
Production      → Langfuse self-hosted or LangSmith
LangChain app   → LangSmith
```

## Exercise
Add DIY logging wrapper to RAG pipeline. Run 5 queries. Check: slowest? Most tokens?
