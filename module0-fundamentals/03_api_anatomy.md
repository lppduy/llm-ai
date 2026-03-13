# API Anatomy

## The Chat Completions API

Standard shape used by OpenAI, Anthropic, OpenRouter, Ollama — nearly universal.

**Request:**
```json
POST /v1/chat/completions
{
  "model": "gpt-4o",
  "messages": [
    {"role": "system", "content": "You are helpful."},
    {"role": "user", "content": "What is 2+2?"}
  ],
  "temperature": 0.0,
  "max_tokens": 100
}
```

**Response:**
```json
{
  "choices": [{
    "message": {"role": "assistant", "content": "4"},
    "finish_reason": "stop"
  }],
  "usage": {
    "prompt_tokens": 15,
    "completion_tokens": 1,
    "total_tokens": 16
  }
}
```

Key fields:
- `messages` — conversation history (you manage this)
- `max_tokens` — cap on output length (not input)
- `usage` — tokens consumed = what you pay for
- `finish_reason` — `"stop"` (done) vs `"length"` (cut off by max_tokens)

## Streaming

Without: user waits 5-10s for full response. With: tokens arrive one at a time.

```json
"stream": true
```

Response becomes SSE (Server-Sent Events):
```
data: {"choices":[{"delta":{"content":"4"}}]}
data: {"choices":[{"delta":{"content":" is"}}]}
data: {"choices":[{"delta":{"content":" the"}}]}
data: {"choices":[{"delta":{"content":" answer"}}]}
data: [DONE]
```

`"delta"` instead of `"message"` — each chunk is just the new piece.

Use streaming for: user-facing chat UI.
Skip for: backend processing, JSON parsing, function calls.

## Conversation Management

API is **stateless** — doesn't remember previous messages. You manage history:

```
Turn 1: [system, user1]                                    → assistant1
Turn 2: [system, user1, assistant1, user2]                  → assistant2
Turn 3: [system, user1, assistant1, user2, assistant2, user3] → assistant3
```

Every turn sends entire history → more tokens → more expensive.
Eventually hits context window → need to truncate or summarize.

This is why chatbot "memory" is hard — not built in.

## Common Parameters

```
model       → "gpt-4o", "claude-sonnet-4-20250514", "llama3"
messages    → conversation history
temperature → randomness (0.0 - 2.0)
max_tokens  → output cap
stream      → true/false
stop        → custom stop sequences (e.g., ["\n\n"])
```
