# Cost Optimization (Caching, Routing, Prompt Compression)

## The 3 Levers
```
1. Caching     — don't call API if seen before
2. Routing     — cheap models when possible, expensive only when needed
3. Compression — send fewer tokens per request
```

## 1. Caching

### Exact Match
```python
import hashlib, json
cache = {}

def cached_llm_call(messages, model="gpt-4o-mini"):
    key = hashlib.md5(json.dumps(messages).encode()).hexdigest()
    if key in cache:
        return cache[key]
    response = client.chat.completions.create(model=model, messages=messages)
    result = response.choices[0].message.content
    cache[key] = result
    return result
```

### Semantic Cache
Cache by meaning. "refund policy?" and "how do I get a refund?" → same answer.
Embed query → search cache by similarity → if > 0.95 → return cached. Libraries: GPTCache, Redis vector search.

### Provider-Level Caching
OpenAI/Anthropic cache repeated system prompts server-side. ~50% savings on cached portion. Automatic for identical prefixes.

## 2. Model Routing

### Tiered Strategy
```
Tier 1: Local (Ollama)    — $0, drafts/simple tasks
Tier 2: Cheap API         — gpt-4o-mini, Haiku (~$0.25/1M)
Tier 3: Smart API         — gpt-4o, Sonnet (~$3/1M)
Tier 4: Premium           — Opus, o1 (~$15/1M)
```

```python
def route_model(query):
    if any(w in query.lower() for w in ["summarize", "translate", "format"]):
        return "gpt-4o-mini"
    if any(w in query.lower() for w in ["analyze", "reason", "compare"]):
        return "gpt-4o"
    return "gpt-4o-mini"  # default cheap
```

## 3. Prompt Compression

### Shorter System Prompts
```
❌ "You are a highly skilled and knowledgeable AI assistant..."
✓  "Medical records assistant. Answer from context only."
```

### Trim Conversation History
```python
def trim_messages(messages, keep_last=6):
    system = [m for m in messages if m["role"] == "system"]
    return system + messages[-keep_last:]
```

### Summarize Old Context
Summarize first N messages into 1 paragraph, send summary + last few messages.

## Savings Table
| Optimization | Effort | Savings |
|-------------|--------|---------|
| Exact cache | Low | 30-80% |
| Model routing | Low | 50-90% |
| Prompt trimming | Low | 20-40% |
| Semantic cache | Medium | 40-70% |

## Side Project Rules
1. Start with gpt-4o-mini or Haiku
2. Cache FAQ-like queries
3. Trim conversation to last 3-5 turns
4. Ollama for dev/testing — $0
5. Log token usage per request

## Exercise
Add exact-match caching to your RAG pipeline. Log cache hits vs misses for 10 queries.
