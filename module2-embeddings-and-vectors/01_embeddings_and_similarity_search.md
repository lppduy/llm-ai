# Embeddings & Similarity Search

## The Problem
Keyword search matches words, not meaning. Semantic search matches meaning.

## What is an Embedding?
Text → list of numbers (vector). Similar meaning → similar numbers.

```
"king"    → [0.21, 0.85, -0.12, ...]   768-1536 dimensions
"queen"   → [0.19, 0.82, -0.10, ...]   (close to king!)
"banana"  → [-0.55, 0.12, 0.78, ...]   (very different)
```

Each dimension captures some aspect of meaning. The embedding model (a neural network) decides the numbers — you just call the API.

## Cosine Similarity
Measures angle between two vectors. Range: -1 to 1. Above 0.8 = usually relevant.

## Token vs Word vs Chunk
- **Token** — smallest unit LLM sees. Never embed alone.
- **Word** — too little context for useful embedding.
- **Chunk** — paragraph/section (200-500 words). This is what you embed for RAG. ✓

## Why Chunks, Not Whole Documents?
Whole doc → blurry average. Chunks → precise, searchable sections.

## Code (Python)
```python
from openai import OpenAI
client = OpenAI()

response = client.embeddings.create(
    model="text-embedding-3-small",
    input="What is Docker?"
)
vector = response.data[0].embedding  # list of 1536 floats
```

With Ollama (free):
```python
client = OpenAI(base_url="http://localhost:11434/v1", api_key="unused")
response = client.embeddings.create(model="nomic-embed-text", input="What is Docker?")
```

## The Pipeline
```
Documents → chunk → embed each chunk → store vectors → query → find closest → return
```
