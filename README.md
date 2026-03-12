# LLM & AI Engineering

Concept-driven, code-walkthrough style. Understand how things work before building with them.

---

## Philosophy

> Don't just call APIs. Understand what's happening behind the abstraction.
> Every tool/framework choice is a tradeoff — know what you're trading and why.

---

## Teaching Style

```
1. Concept     — what, why, when
2. How it works — internals, not just API surface
3. Code walkthrough — working sample, explain each part
4. Tradeoffs   — alternatives, when NOT to use
5. Exercise    — small task to solidify
```

---

## Progress Tracking

```
[ ]  not started
[~]  in progress
[x]  concept + code walkthrough done
```

---

## Module 0: LLM Fundamentals

Quick primers — cover these before touching any framework.

| # | Topic | Status |
|---|-------|--------|
| 01 | How LLMs work (tokens, context windows, temperature) | [x] |
| 02 | Prompt engineering patterns (roles, few-shot, CoT) | [ ] |
| 03 | API anatomy (completions, chat, streaming) | [ ] |
| 04 | Local models with Ollama | [ ] |

## Module 1: APIs & Model Routing

Calling LLMs from code. Multiple providers, unified access.

| # | Topic | Status |
|---|-------|--------|
| 01 | OpenAI API (Python + TS) | [ ] |
| 02 | Anthropic API (Python + TS) | [ ] |
| 03 | OpenRouter — unified multi-model API | [ ] |
| 04 | Model selection strategy (cost vs quality vs speed) | [ ] |

## Module 2: Embeddings & Vector Search

Turn text into numbers. Search by meaning, not keywords.

| # | Topic | Status |
|---|-------|--------|
| 01 | What embeddings are + similarity search | [ ] |
| 02 | Embedding models (local vs cloud) | [ ] |
| 03 | ChromaDB — store, query, filter | [ ] |
| 04 | Chunking strategies | [ ] |

## Module 3: RAG (Retrieval-Augmented Generation)

Give LLMs access to your data without fine-tuning.

| # | Topic | Status |
|---|-------|--------|
| 01 | RAG pipeline end-to-end | [ ] |
| 02 | RAG with LlamaIndex | [ ] |
| 03 | RAG with LangChain | [ ] |
| 04 | Hybrid search + reranking | [ ] |
| 05 | Source citations + hallucination reduction | [ ] |

## Module 4: Agents & Tool Use

LLMs that can take actions, not just generate text.

| # | Topic | Status |
|---|-------|--------|
| 01 | Tool/function calling (OpenAI + Anthropic format) | [ ] |
| 02 | ReAct pattern (Reason → Act → Observe) | [ ] |
| 03 | Building agents with LangGraph | [ ] |
| 04 | Multi-agent systems | [ ] |
| 05 | When agents make sense vs overkill | [ ] |

## Module 5: Production & Integration

Ship it. Cost, quality, observability.

| # | Topic | Status |
|---|-------|--------|
| 01 | Cost optimization (caching, routing, prompt compression) | [ ] |
| 02 | Evaluation — measuring LLM app quality | [ ] |
| 03 | Observability (Langfuse / LangSmith) | [ ] |
| 04 | Guardrails (input validation, output filtering) | [ ] |
| 05 | Deployment patterns (FastAPI + Next.js + LLM) | [ ] |

---

## Tool/Framework Reference

| Category | Primary | Alternative |
|----------|---------|-------------|
| Local models | Ollama | LM Studio |
| Model routing | OpenRouter | LiteLLM |
| Python SDK | openai, anthropic | litellm |
| TS SDK | Vercel AI SDK | LangChain.js |
| RAG framework | LlamaIndex | LangChain |
| Agent framework | LangGraph | CrewAI |
| Vector DB | ChromaDB (local) | Pinecone (cloud) |
| Observability | Langfuse (self-host) | LangSmith |
