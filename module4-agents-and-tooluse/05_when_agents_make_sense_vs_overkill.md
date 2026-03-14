# When Agents Make Sense vs Overkill

## The Ladder — Start at the Bottom
```
Level 0: Prompt only          → "Summarize this text"
Level 1: Prompt + RAG         → "Answer from my docs"
Level 2: Single tool call     → "What's the weather?"
Level 3: ReAct loop           → "Compare weather in 3 cities"
Level 4: Single agent         → "Research, find data, write report"
Level 5: Multi-agent          → "Team of agents collaborating"
```
**Rule: Use the lowest level that solves your problem.**

## When Each Level Fits
- **Level 0-1 (80% of apps):** Chatbot, Q&A, summarization, RAG knowledge base
- **Level 2-3 (15%):** External data lookup, API calls, 1-5 tool calls
- **Level 4-5 (5%):** Complex workflows, distinct phases, coding assistants

## Real Examples
| Use Case | Level | Why |
|----------|-------|-----|
| FAQ chatbot | 0 | Just prompt engineering |
| Med records Q&A | 1 | RAG, no tools needed |
| "Book me a flight" | 3 | Search + booking + confirm |
| Code review bot | 4 | Read + lint + test + write review |
| Research paper writer | 5 | Researcher + writer + reviewer |

## Red Flags — Over-engineering
1. Agent for static knowledge — just use RAG
2. Agent with 1 tool — call it directly
3. Multi-agent when `if/else` works — simpler is better
4. "Could be useful later" — YAGNI
5. Agent calling agent calling agent — if you can't explain it, simplify

## Cost Reality
```
Single prompt:  ~$0.001  (1 call)
RAG pipeline:   ~$0.005  (embed + retrieve + generate)
Agent (5 tools): ~$0.02  (5-10 calls)
Multi-agent:    ~$0.10+  (each agent = multiple calls)
```

## Decision Flowchart
```
Need external data/actions?
  No  → Level 0-1
  Yes → How many steps?
    1     → Level 2
    2-5   → Level 3
    5+ with branching → Level 4
    Distinct roles    → Level 5
```

## Tradeoffs
| | Prompt/RAG | Agent | Multi-Agent |
|--|-----------|-------|-------------|
| Cost | Low | Medium | High |
| Latency | Fast | Medium | Slow |
| Reliability | High | Medium | Lower |
| Debugging | Easy | Moderate | Hard |

## Exercise
For your med records chatbot: (1) what level to start, (2) what triggers upgrade, (3) what level you'd never need and why.
