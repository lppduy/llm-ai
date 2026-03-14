# CLAUDE.md

## Context
LLM & AI Engineering study session, concept + code walkthrough, Python + TypeScript.

## Teaching Style
- Explain the concept first — what it is, why it exists, what problem it solves
- Walk through sample code together (Python primary, TS secondary)
- After each concept: small exercise or modification to solidify understanding
- Go one topic at a time — never skip ahead
- Use free/local tools when possible (Ollama, OpenRouter free models, free tiers)

## Session Format
1. Concept    — what is it, why does it matter, when to use it
2. How it works — internals, not just the API surface
3. Code walkthrough — working sample code, explain each part
4. Tradeoffs  — alternatives, when NOT to use this approach
5. Exercise   — small modification or extension to practice

## Stack
- Python 3.12+ (openai, anthropic, langchain, llama-index SDKs)
- TypeScript/Node (openai, @anthropic-ai/sdk, ai from Vercel)
- Ollama for local models (Llama 3, Mistral, embeddings)
- OpenRouter for multi-model routing
- ChromaDB for vector storage (local)

## Budget Strategy
- Weeks 1-3: 100% local with Ollama — $0
- Weeks 4-5: OpenRouter free/cheap models — ~$2-5
- Week 6+: Small API spend for testing — ~$5

## Progress Tracking
```
[ ]  not started
[~]  in progress
[x]  concept + code walkthrough done
```

## Notes
- After each topic is fully covered: create the .md file, update progress in CLAUDE.md and README.md, commit AND push — without waiting for user to ask
- Always push immediately after committing — never leave commits unpushed

## Progress

### Module 0: LLM Fundamentals
- [x] M0-01 How LLMs work (tokens, context windows, temperature)
- [x] M0-02 Prompt engineering patterns (roles, few-shot, CoT)
- [x] M0-03 API anatomy (completions, chat, streaming)
- [x] M0-04 Local models with Ollama

### Module 1: APIs & Model Routing
- [x] M1-01 OpenAI API (Python + TS)
- [x] M1-02 Anthropic API (Python + TS)
- [x] M1-03 OpenRouter — unified multi-model API
- [x] M1-04 Model selection strategy (cost vs quality vs speed)

### Module 2: Embeddings & Vector Search
- [x] M2-01 What embeddings are + similarity search
- [x] M2-02 Embedding models (local vs cloud)
- [x] M2-03 ChromaDB — store, query, filter
- [x] M2-04 Chunking strategies

### Module 3: RAG
- [ ] M3-01 RAG pipeline end-to-end
- [ ] M3-02 RAG with LlamaIndex
- [ ] M3-03 RAG with LangChain
- [ ] M3-04 Hybrid search + reranking
- [ ] M3-05 Source citations + hallucination reduction

### Module 4: Agents & Tool Use
- [ ] M4-01 Tool/function calling (OpenAI + Anthropic format)
- [ ] M4-02 ReAct pattern (Reason → Act → Observe)
- [ ] M4-03 Building agents with LangGraph
- [ ] M4-04 Multi-agent systems
- [ ] M4-05 When agents make sense vs overkill

### Module 5: Production & Integration
- [ ] M5-01 Cost optimization (caching, routing, prompt compression)
- [ ] M5-02 Evaluation — measuring LLM app quality
- [ ] M5-03 Observability (Langfuse / LangSmith)
- [ ] M5-04 Guardrails (input validation, output filtering)
- [ ] M5-05 Deployment patterns (FastAPI + Next.js + LLM)

## Resume
Module 2 complete. Next: M3-01 RAG pipeline end-to-end.
