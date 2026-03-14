# ReAct Pattern (Reason → Act → Observe)

## What Is ReAct?
A loop where the LLM: thinks → calls a tool → reads result → repeats until it can answer. Foundation of every AI agent.

## Why It Matters
One-shot tool calling = single lookup. ReAct = multi-step reasoning with chained tool calls.

```
User: "Is it warmer in Tokyo or Paris?"
LLM thinks: need both cities
  → Act: get_weather("Tokyo") → Observe: 22°C
  → Act: get_weather("Paris") → Observe: 18°C
  → Reason: Tokyo is warmer → Answer
```

## The Loop
```
while not done:
    1. Send conversation + tool results to LLM
    2. If LLM returns text → done
    3. If LLM returns tool_call → execute it
    4. Append tool result to conversation
    5. Go to step 1
```

## Code — ReAct Agent
```python
import openai, json

client = openai.OpenAI()

# Tools + implementations
def get_weather(city):
    data = {"Tokyo": "22°C sunny", "Paris": "18°C cloudy"}
    return data.get(city, "Unknown city")

tool_map = {"get_weather": get_weather}

tools = [{"type": "function", "function": {
    "name": "get_weather",
    "description": "Get weather for a city",
    "parameters": {"type": "object", "properties": {
        "city": {"type": "string"}
    }, "required": ["city"]}
}}]

# THE LOOP
def react_agent(user_message, max_iter=10):
    messages = [
        {"role": "system", "content": "Use tools when needed."},
        {"role": "user", "content": user_message}
    ]

    for _ in range(max_iter):
        response = client.chat.completions.create(
            model="gpt-4o-mini", messages=messages, tools=tools
        )
        msg = response.choices[0].message

        if not msg.tool_calls:
            return msg.content  # done

        messages.append(msg)
        for tc in msg.tool_calls:
            fn_name = tc.function.name
            fn_args = json.loads(tc.function.arguments)
            result = tool_map[fn_name](**fn_args)
            messages.append({
                "role": "tool",
                "tool_call_id": tc.id,
                "content": str(result)
            })

    return "Max iterations reached"
```

## Key Insights
1. **Loop is yours** — LLM doesn't loop itself, your code does
2. **Conversation grows** — each tool call + result appended to messages
3. **LLM decides when to stop** — returns text instead of tool_call
4. **Always set max iterations** — prevent infinite loops

## One-Shot vs ReAct
| | One-Shot | ReAct |
|--|----------|-------|
| Tool calls | 1 | Multiple, chained |
| Decision | Pick tool once | Reason after each result |
| Use case | Simple lookup | Multi-step problems |

## Tradeoffs
- More API calls = higher cost + latency
- Context grows each iteration (more tokens)
- Must have safety limits (max iterations)
- This is what LangChain/LangGraph wrap with extra features

## Exercise
Add `calculate(expression)` tool. Ask: "Temperature difference between Tokyo and Paris in Fahrenheit?" — agent should get both temps, convert, then calculate.
