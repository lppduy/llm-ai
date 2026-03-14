# Deployment Patterns (FastAPI + Next.js + LLM)

## Architecture
```
Frontend (Next.js) → Backend (FastAPI) → LLM API (OpenAI/..)
                          ↓
                     Vector DB (ChromaDB)
```
Never call LLM APIs from frontend — exposes API keys.

## Backend — FastAPI
```python
from fastapi import FastAPI
from fastapi.responses import StreamingResponse
from pydantic import BaseModel
import openai

app = FastAPI()
client = openai.OpenAI()

class ChatRequest(BaseModel):
    message: str
    session_id: str = ""

@app.post("/api/chat")
async def chat(req: ChatRequest):
    chunks = retrieve(req.message)
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {"role": "system", "content": f"Context: {chunks}"},
            {"role": "user", "content": req.message}
        ]
    )
    return {"answer": response.choices[0].message.content}

# Streaming
@app.post("/api/chat/stream")
async def chat_stream(req: ChatRequest):
    chunks = retrieve(req.message)
    def generate():
        stream = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[
                {"role": "system", "content": f"Context: {chunks}"},
                {"role": "user", "content": req.message}
            ], stream=True)
        for chunk in stream:
            if chunk.choices[0].delta.content:
                yield f"data: {chunk.choices[0].delta.content}\n\n"
        yield "data: [DONE]\n\n"
    return StreamingResponse(generate(), media_type="text/event-stream")
```

## Frontend — Next.js + Vercel AI SDK
```typescript
// app/page.tsx
'use client';
import { useChat } from 'ai/react';

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();
  return (
    <div>
      {messages.map(m => (
        <div key={m.id}><b>{m.role}:</b> {m.content}</div>
      ))}
      <form onSubmit={handleSubmit}>
        <input value={input} onChange={handleInputChange} />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}
```

## Deployment Options
| Option | Cost | Best For |
|--------|------|----------|
| Railway / Render | $5-20/mo | Side projects |
| Vercel + Railway | Free-$20/mo | Next.js + Python |
| Docker + VPS | $5-10/mo | Full control |
| AWS/GCP | Variable | Production scale |

## Key Patterns
1. **Never expose API keys** — keys on server only
2. **Rate limiting** — `slowapi` or similar
3. **Background processing** — heavy tasks (embedding) async
4. **Health check** — `/health` endpoint
5. **Streaming** — SSE for real-time responses

## Med Records Stack
```
Frontend: Next.js (chat UI, file upload)
Backend:  FastAPI (RAG, guardrails)
Vector:   ChromaDB (per-user collections)
LLM:      gpt-4o-mini via OpenRouter
Deploy:   Docker on Railway or VPS
```

## Exercise
Create minimal FastAPI + Next.js: one `/api/chat` endpoint, one chat page. Send message, get response. No RAG — just LLM call.
