# RAG Pipeline End-to-End

## What is RAG?
Retrieval-Augmented Generation — fetch relevant docs, inject into prompt, LLM answers using YOUR data.

## The Pipeline
```
User question → embed → search vector DB → top K chunks → build prompt → LLM → answer
```

## RAG vs Fine-tuning
- RAG: cheap, updatable, cites sources. Wins 90% of cases.
- Fine-tuning: expensive, hard to update, may hallucinate.

## Code — From Scratch
```python
from openai import OpenAI
import chromadb

llm = OpenAI()
chroma = chromadb.PersistentClient(path="./rag_db")
collection = chroma.get_or_create_collection("docs")

# Index (once)
collection.add(
    ids=["doc0", "doc1"],
    documents=["Refunds available within 30 days.", "Contact support@company.com."]
)

# Query (per question)
question = "How do I get a refund?"
results = collection.query(query_texts=[question], n_results=2)
context = "\n".join(results["documents"][0])

response = llm.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "system", "content": f"Answer from context:\n{context}\nIf not found, say so."},
        {"role": "user", "content": question}
    ]
)
print(response.choices[0].message.content)
```
