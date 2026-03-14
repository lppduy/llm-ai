# RAG with LlamaIndex

## What is it?
Framework that automates RAG pipeline. 30 lines from scratch → 5 lines.

## Setup
```bash
pip install llama-index
```

## Basic RAG — 5 Lines
```python
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader

documents = SimpleDirectoryReader("./my_docs").load_data()
index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine()
response = query_engine.query("What is the refund policy?")
```

## Under the Hood
```
SimpleDirectoryReader → loads files (PDF, TXT, MD)
VectorStoreIndex      → chunks → embeds → stores
as_query_engine()     → retriever + prompt builder + LLM
.query()              → embed question → search → prompt → LLM
```

## Customize
```python
from llama_index.core import Settings
from llama_index.llms.openai import OpenAI
Settings.llm = OpenAI(model="gpt-4o-mini", temperature=0)
Settings.chunk_size = 512
Settings.chunk_overlap = 50
```

## When to Use
- Prototyping, simple RAG → LlamaIndex
- Need full control → raw approach (M3-01)
