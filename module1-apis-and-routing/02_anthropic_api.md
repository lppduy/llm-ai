# Anthropic API (Python + TS)

## Key Difference from OpenAI
System message is a **separate parameter**, not inside messages array. `max_tokens` is required.

## Setup
```bash
pip install anthropic              # Python
npm install @anthropic-ai/sdk      # TS
export ANTHROPIC_API_KEY="sk-ant-..."
```

## Python
```python
import anthropic
client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    system="You are a helpful assistant.",
    messages=[{"role": "user", "content": "Explain Docker in one sentence."}]
)
print(response.content[0].text)
print(f"Tokens: {response.usage.input_tokens} in, {response.usage.output_tokens} out")
```

## TypeScript
```typescript
import Anthropic from "@anthropic-ai/sdk";
const client = new Anthropic();

const response = await client.messages.create({
  model: "claude-sonnet-4-20250514",
  max_tokens: 1024,
  system: "You are a helpful assistant.",
  messages: [{ role: "user", content: "Explain Docker in one sentence." }],
});
console.log(response.content[0].text);
```

## OpenAI vs Anthropic Comparison

| | OpenAI | Anthropic |
|--|--------|-----------|
| Method | `chat.completions.create()` | `messages.create()` |
| System msg | Inside `messages` array | Separate `system` param |
| Response | `.choices[0].message.content` | `.content[0].text` |
| max_tokens | Optional | **Required** |
| Env var | `OPENAI_API_KEY` | `ANTHROPIC_API_KEY` |

Use OpenRouter (M1-03) for unified API across all providers.
