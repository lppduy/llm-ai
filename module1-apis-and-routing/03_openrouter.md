# OpenRouter — Unified Multi-Model API

## What is OpenRouter?
One API key → hundreds of models (OpenAI, Anthropic, Google, Meta, Mistral). Uses OpenAI-compatible format.

Why: no separate accounts, easy model switching, some free models, fallback routing.

## Setup
```bash
export OPENROUTER_API_KEY="sk-or-..."
```

## Python — Just the OpenAI SDK
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://openrouter.ai/api/v1",
    api_key="sk-or-..."
)

response = client.chat.completions.create(
    model="anthropic/claude-sonnet-4-20250514",   # provider/model format
    messages=[{"role": "user", "content": "What is Docker?"}]
)
print(response.choices[0].message.content)
```

Same `openai` SDK, different `base_url`. No OpenAI account needed.

## Model Naming
```
provider/model-name
  openai/gpt-4o
  anthropic/claude-sonnet-4-20250514
  google/gemini-2.0-flash-exp
  meta-llama/llama-3.1-8b-instruct
```

## Free Models
Append `:free` — rate-limited but $0:
```
meta-llama/llama-3.1-8b-instruct:free
google/gemma-2-9b-it:free
mistralai/mistral-7b-instruct:free
```

## OpenAI Format = Universal Standard
`openai` SDK is just an HTTP client that speaks the OpenAI API format. Many providers support it:
- OpenAI (invented it), Ollama, OpenRouter, Together AI, Groq
- Anthropic and Google have their **own** SDK/format

Think USB cable — invented by one company, used by everyone.

## Model Switching
Change one string, everything else stays the same:
```python
MODEL = "meta-llama/llama-3.1-8b-instruct:free"  # or "openai/gpt-4o"
response = client.chat.completions.create(model=MODEL, messages=[...])
```
