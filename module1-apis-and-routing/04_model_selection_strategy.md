# Model Selection Strategy

## The 3 Axes
Every model choice trades between: **Quality ←→ Cost ←→ Speed**. Pick 2.

## Decision Matrix

| Task | Best choice | Why |
|------|------------|-----|
| Classification, extraction | Llama 8B / Mistral 7B | Cheap, fast, good enough |
| General chat, Q&A | GPT-4o-mini / Gemini Flash | Balanced cost + quality |
| Complex reasoning, code | GPT-4o / Claude Sonnet | Worth paying for accuracy |
| Hardest problems | Claude Opus / GPT-4o | Best quality, most expensive |

## Practical Rule
Start cheap, upgrade only when quality fails:
1. Try free/cheap model (Llama, Mistral)
2. Quality bad → mid-tier (GPT-4o-mini)
3. Still bad → top-tier (GPT-4o, Claude Sonnet)
4. Only Opus for truly hard tasks

## Side Project Recommendations
```
User-facing chat     → GPT-4o-mini ($0.15/1M in)
RAG retrieval        → Llama 8B (free) — matching, not reasoning
Code generation      → Claude Sonnet ($3/1M in) — best for code
Classification       → Mistral 7B (free)
```
