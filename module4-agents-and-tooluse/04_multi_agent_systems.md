# Multi-Agent Systems

## What Is It?
Multiple specialized agents working together, each with own role, tools, expertise. Split work instead of one agent doing everything.

## Why Multi-Agent?
Single agent problems: too many tools → confused, context fills fast, poor at everything.

## Patterns

### 1. Supervisor
Boss agent routes tasks to workers.
```
User → Supervisor → Agent A → Supervisor → Agent B → User
```

### 2. Sequential (Pipeline)
Agents pass work in order.
```
User → Researcher → Writer → Reviewer → User
```

### 3. Debate
Agents refine each other's output.
```
Agent A proposes → Agent B critiques → Agent A revises → done
```

## Code — Simple Pipeline (No Framework)
```python
import openai
client = openai.OpenAI()

def multi_agent_pipeline(query):
    # Agent 1: Research
    research = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {"role": "system", "content": "You are a researcher. List key facts."},
            {"role": "user", "content": query}
        ]
    ).choices[0].message.content

    # Agent 2: Write
    summary = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {"role": "system", "content": "You are a writer. Summarize clearly."},
            {"role": "user", "content": f"Research:\n{research}\n\nWrite a summary."}
        ]
    ).choices[0].message.content

    return summary
```

## Code — Supervisor with LangGraph
```python
from langgraph.graph import StateGraph, START, END
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage, SystemMessage
from typing import Annotated, TypedDict
from langgraph.graph.message import add_messages

class State(TypedDict):
    messages: Annotated[list, add_messages]
    next_agent: str

researcher_llm = ChatOpenAI(model="gpt-4o-mini")
writer_llm = ChatOpenAI(model="gpt-4o-mini")

def researcher(state: State):
    msgs = [SystemMessage("Find key facts only.")] + state["messages"]
    return {"messages": [researcher_llm.invoke(msgs)]}

def writer(state: State):
    msgs = [SystemMessage("Write clear summary from research.")] + state["messages"]
    return {"messages": [writer_llm.invoke(msgs)]}

def supervisor(state: State):
    if len(state["messages"]) <= 2:
        return {"next_agent": "researcher"}
    return {"next_agent": "writer"}

graph = StateGraph(State)
graph.add_node("supervisor", supervisor)
graph.add_node("researcher", researcher)
graph.add_node("writer", writer)
graph.add_edge(START, "supervisor")
graph.add_conditional_edges("supervisor", lambda s: s["next_agent"],
    {"researcher": "researcher", "writer": "writer"})
graph.add_edge("researcher", "supervisor")
graph.add_edge("writer", END)
app = graph.compile()
```

## Frameworks
| Framework | Style | Best For |
|-----------|-------|----------|
| LangGraph | Graph-based | Complex routing, state |
| CrewAI | Role-based | Quick setup, pre-defined roles |
| AutoGen | Conversation | Agents chat with each other |
| Raw code | Pipeline | Simple sequential chains |

## When to Use
- **Yes:** distinct phases, >10 tools, different models per task
- **No:** simple Q&A with 2-3 tools, no clear benefit, "sounds cool"

## Tradeoffs
- More API calls = higher cost
- Coordination overhead — agents can misunderstand each other
- Debugging harder — which agent went wrong?
- Latency = sum of all agent response times

## Exercise
Build 2-agent pipeline: Agent 1 finds 3 facts, Agent 2 writes a paragraph. Raw OpenAI calls, no framework.
