# Tool/Function Calling (OpenAI + Anthropic Format)

## What It Is
LLM decides which function to call and with what arguments. YOU execute it, return result, LLM continues.

## Why It Matters
Without tools, LLMs can only generate text. With tools, they can: search databases, call APIs, do math, read files, take actions.

## How It Works
```
1. You define available tools (name, description, parameters)
2. Send user message + tool definitions to LLM
3. LLM returns: "call function X with args Y" (NOT text)
4. You execute the function locally
5. Send result back to LLM
6. LLM generates final answer using the result
```

**Key insight:** LLM never executes code. It only decides WHAT to call. You run it.

## OpenAI Format
```python
import openai

tools = [
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get current weather for a city",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {"type": "string", "description": "City name"},
                    "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]}
                },
                "required": ["city"]
            }
        }
    }
]

response = openai.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": "Weather in Tokyo?"}],
    tools=tools
)

# response.choices[0].message.tool_calls[0]
# → function.name = "get_weather"
# → function.arguments = '{"city": "Tokyo"}'
```

### Full Loop
```python
msg = response.choices[0].message

if msg.tool_calls:
    # YOU execute the function
    result = get_weather(city="Tokyo")

    # Send result back
    messages.append(msg)  # assistant's tool_call message
    messages.append({
        "role": "tool",
        "tool_call_id": msg.tool_calls[0].id,
        "content": json.dumps(result)
    })

    # LLM generates final answer
    final = openai.chat.completions.create(
        model="gpt-4o-mini",
        messages=messages,
        tools=tools
    )
    print(final.choices[0].message.content)
```

## Anthropic Format
```python
import anthropic

response = anthropic.Anthropic().messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    tools=[{
        "name": "get_weather",
        "description": "Get current weather for a city",
        "input_schema": {
            "type": "object",
            "properties": {
                "city": {"type": "string"},
                "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]}
            },
            "required": ["city"]
        }
    }],
    messages=[{"role": "user", "content": "Weather in Tokyo?"}]
)

# response.content[0].type == "tool_use"
# response.content[0].name == "get_weather"
# response.content[0].input == {"city": "Tokyo"}
```

## OpenAI vs Anthropic Differences

| | OpenAI | Anthropic |
|--|--------|-----------|
| Key | `parameters` | `input_schema` |
| Response | `tool_calls[0].function` | `content[0]` where type="tool_use" |
| Result role | `"tool"` | `"user"` with tool_result content |
| ID field | `tool_call_id` | `tool_use_id` |

## Multiple Tools
Define array of tools. LLM picks the right one (or none) based on the query. Can call multiple tools in one turn.

## When to Use
- Database lookups, API calls, calculations
- Any time LLM needs real-time or external data
- Actions: send email, create ticket, update record

## Exercise
Define a `calculate` tool (operation, a, b) and a `search_docs` tool. Send "What's 42 * 17?" — verify LLM picks `calculate`.
