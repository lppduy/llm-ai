# OpenAI API (Python + TS)

## Setup
```bash
pip install openai       # Python
npm install openai       # TS
export OPENAI_API_KEY="sk-..."
```

## Python
```python
from openai import OpenAI
client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Explain Docker in one sentence."}
    ],
    temperature=0.0
)
print(response.choices[0].message.content)
print(f"Tokens: {response.usage.total_tokens}")
```

## Streaming (Python)
```python
stream = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Explain Docker."}],
    stream=True
)
for chunk in stream:
    content = chunk.choices[0].delta.content
    if content:
        print(content, end="", flush=True)
```

## TypeScript
```typescript
import OpenAI from "openai";
const client = new OpenAI();

const response = await client.chat.completions.create({
  model: "gpt-4o",
  messages: [
    { role: "system", content: "You are a helpful assistant." },
    { role: "user", content: "Explain Docker in one sentence." },
  ],
  temperature: 0.0,
});
console.log(response.choices[0].message.content);
```

## Works with Any OpenAI-compatible Provider
```python
# Ollama
client = OpenAI(base_url="http://localhost:11434/v1", api_key="unused")
# OpenRouter
client = OpenAI(base_url="https://openrouter.ai/api/v1", api_key="or-...")
```
