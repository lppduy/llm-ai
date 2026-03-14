# Building Agents with LangGraph

## What Is LangGraph?
Framework by LangChain team for building stateful, multi-step agents as graphs. Nodes = functions, Edges = connections, State = shared data.

## Why Over Raw Loop?
| Raw ReAct Loop | LangGraph |
|----------------|-----------|
| Simple while loop | Graph with nodes + edges |
| Manual state | Built-in state management |
| Hard to branch | Conditional edges easy |
| No persistence | Built-in checkpointing |

## Install
```bash
pip install langgraph langchain-openai
```

## Code — Basic Agent
```python
from langgraph.graph import StateGraph, START, END
from langgraph.prebuilt import ToolNode
from langchain_openai import ChatOpenAI
from langchain_core.tools import tool
from typing import Annotated, TypedDict
from langgraph.graph.message import add_messages

# 1. State
class AgentState(TypedDict):
    messages: Annotated[list, add_messages]

# 2. Tools
@tool
def get_weather(city: str) -> str:
    """Get weather for a city."""
    data = {"Tokyo": "22°C sunny", "Paris": "18°C cloudy"}
    return data.get(city, "Unknown")

@tool
def calculate(expression: str) -> str:
    """Calculate a math expression."""
    return str(eval(expression))

tools = [get_weather, calculate]
llm = ChatOpenAI(model="gpt-4o-mini").bind_tools(tools)

# 3. Nodes
def call_llm(state: AgentState):
    return {"messages": [llm.invoke(state["messages"])]}

tool_node = ToolNode(tools)

# 4. Routing
def should_continue(state: AgentState):
    if state["messages"][-1].tool_calls:
        return "tools"
    return END

# 5. Build Graph
graph = StateGraph(AgentState)
graph.add_node("llm", call_llm)
graph.add_node("tools", tool_node)
graph.add_edge(START, "llm")
graph.add_conditional_edges("llm", should_continue, {"tools": "tools", END: END})
graph.add_edge("tools", "llm")

agent = graph.compile()

# 6. Run
result = agent.invoke({
    "messages": [{"role": "user", "content": "Is Tokyo warmer than Paris?"}]
})
print(result["messages"][-1].content)
```

## The Flow
```
START → LLM → has tool_calls? → yes → Tools → back to LLM
                               → no  → END
```

## Key Concepts
- `@tool` decorator — auto-generates schema from type hints + docstring
- `ToolNode` — pre-built node that executes tool calls
- `add_messages` — appends messages instead of replacing
- Conditional edges — route to different nodes based on state

## When LangGraph Shines
- Human-in-the-loop (pause, ask approval, resume)
- Checkpointing (save state, resume later)
- Branching (different paths per LLM decision)
- Subgraphs (nest graphs for complex workflows)

## When to Use What
```
Simple 1-2 tool agent     → raw ReAct loop
Complex multi-step agent  → LangGraph
Multi-agent orchestration → LangGraph
Just need RAG             → LlamaIndex, skip agents
```

## Exercise
Add `search_docs` tool. Ask: "Find docs about refund policy, summarize in 2 sentences." Verify graph routes through tools then back to LLM.
