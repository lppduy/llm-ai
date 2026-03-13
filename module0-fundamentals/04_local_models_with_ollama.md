# Local Models with Ollama

## What is Ollama?
Run LLMs locally on your Mac. No API key, no internet, no cost. Serves via OpenAI-compatible API on `localhost:11434`.

Why it matters:
- Free experimentation
- Privacy — data never leaves machine
- Same API format as OpenAI → code works with both

## Setup
```bash
brew install ollama
ollama serve              # start server
ollama pull llama3.2      # 2GB, good for learning
ollama pull mistral       # 4GB, better quality
```

## The API

CLI:
```bash
ollama run llama3.2 "What is Docker?"
```

API (OpenAI-compatible):
```bash
curl http://localhost:11434/v1/chat/completions \
  -d '{"model": "llama3.2", "messages": [{"role": "user", "content": "What is Docker?"}]}'
```

## Using with OpenAI SDK

```python
from openai import OpenAI

# OpenAI cloud
client = OpenAI(api_key="sk-...")

# Ollama local — SAME CODE, different base_url
client = OpenAI(base_url="http://localhost:11434/v1", api_key="unused")

response = client.chat.completions.create(
    model="llama3.2",
    messages=[{"role": "user", "content": "What is Docker?"}]
)
print(response.choices[0].message.content)
```

OpenAI SDK = "lingua franca". Most providers (OpenRouter, Ollama, Together AI) support its format.

## Recommended Models

| Model | Size | Good for |
|-------|------|----------|
| llama3.2 | 2GB | Learning, fast |
| mistral | 4GB | Better quality |
| llama3.1:8b | 4.7GB | Strong general purpose |
| codellama | 3.8GB | Code tasks |
| nomic-embed-text | 274MB | Embeddings (Module 2) |

## Ollama vs Cloud

| | Ollama | Cloud API |
|--|--------|-----------|
| Cost | $0 | Pay per token |
| Quality | 7-8B = okay | GPT-4o/Claude = much better |
| Privacy | Local | Sent to provider |
| Internet | Not needed | Required |

Start with Ollama for learning. Switch to cloud when you need better quality.
