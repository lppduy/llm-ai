# How LLMs Work

## What Is an LLM?
A **next-token prediction machine**. Given a sequence of tokens, predicts the most likely next token. Repeat thousands of times → coherent text.

```
"The cat sat on the" → [mat: 0.35, floor: 0.20, roof: 0.08, ...]
                         ↑ pick highest probability
```

Training: read billions of documents, learned statistical patterns. Sophisticated pattern matching, not human understanding.

## Tokens ≠ Words

LLMs see **tokens** — subword chunks, not words.

```
"Hello world"     → ["Hello", " world"]           = 2 tokens
"tokenization"    → ["token", "ization"]           = 2 tokens
"unhappiness"     → ["un", "happiness"]            = 2 tokens
"Xin chào"        → ["X", "in", " ch", "ào"]      = 4 tokens (non-English = more tokens)
```

**Rule of thumb:** 1 token ≈ 4 characters ≈ 0.75 English words.

Why it matters:
- You pay per token (input + output)
- Context window measured in tokens
- Non-English text costs more tokens for same meaning

## Context Window

Max tokens the model can "see" at once (input + output combined). Think of it as RAM.

```
GPT-4o:          128K tokens (~96K words)
Claude Sonnet:   200K tokens (~150K words)
Llama 3 8B:      8K tokens  (~6K words)
Mistral 7B:      32K tokens (~24K words)
```

Anything outside context window doesn't exist to the model.

## Temperature

Controls randomness in token selection.

```
temp=0.0 → always pick highest probability (deterministic)
temp=0.7 → sometimes pick lower probability (creative)
temp=1.0 → very random, can be incoherent
```

When to use:
- `0.0` — code generation, factual Q&A, structured output (JSON)
- `0.3-0.7` — conversation, creative writing
- `0.8+` — brainstorming, poetry

## Top-p (Nucleus Sampling)

"Only consider tokens whose cumulative probability reaches p%."

```
top_p=0.1 → only top 10% most likely tokens
top_p=0.9 → tokens covering 90% of probability mass
```

Pick temperature OR top-p, not both. Temperature is more intuitive.

## Transformer Architecture (30-second version)

```
Input text
    ↓
[Tokenizer] → text to token IDs
    ↓
[Embedding] → token ID → dense vector (e.g., 4096 dims)
    ↓
[Transformer blocks × N] → self-attention + feed-forward
  │   (attention = "which tokens matter for predicting next one")
    ↓
[Output] → probability distribution over all possible next tokens
    ↓
Pick one token → append to input → repeat (AUTOREGRESSIVE)
```

500-word response = ~375 forward passes through entire model.

## Small vs Large Models

| Factor | Small (7B) | Large (70B+) |
|--------|-----------|-------------|
| Speed | Fast | Slow |
| Cost | Cheap/free (local) | Expensive (API) |
| Quality | Good for simple tasks | Better reasoning |
| Context | Usually smaller | Usually larger |
| Privacy | Runs locally | Data goes to provider |

**Small:** classification, extraction, simple Q&A, privacy-sensitive
**Large:** complex reasoning, creative tasks, multi-step problems

## Key Numbers

```
1 token           ≈ 4 chars ≈ 0.75 words
1,000 tokens      ≈ 750 words ≈ 1.5 pages
GPT-4o            ≈ $2.50/1M input, $10/1M output
Claude Sonnet     ≈ $3/1M input, $15/1M output
Ollama (local)    = $0
```

## Exercise

```bash
brew install ollama
ollama serve          # terminal 1
ollama pull llama3.2  # terminal 2
ollama run llama3.2
```

Try:
1. Same question 3x — answers vary (temperature > 0)
2. "Count letters in strawberry" — struggles (tokenization)
3. Paste long paragraph, ask about details — works (context window)
4. Ask in Vietnamese — slower/less accurate (more tokens per word)
