# ChromaDB — Store, Query, Filter

## What is it?
Vector database. Stores embeddings, searches by similarity. Runs locally, no server.

## Setup
```bash
pip install chromadb
```

## Add Documents
```python
import chromadb
client = chromadb.Client()
collection = client.create_collection("my_docs")

collection.add(
    ids=["doc1", "doc2", "doc3"],
    documents=[
        "Docker is a containerization platform",
        "Kubernetes orchestrates containers at scale",
        "Python is a programming language"
    ]
)
```

## Query by Similarity
```python
results = collection.query(
    query_texts=["how to deploy containers"],
    n_results=2
)
# Returns Docker and K8s docs. Python doc excluded — not related.
```

## Metadata Filtering
```python
collection.add(
    ids=["doc4"], documents=["Docker networking basics"],
    metadatas=[{"topic": "networking", "level": "beginner"}]
)

results = collection.query(
    query_texts=["container network"],
    where={"level": "beginner"}
)
```

## Persistent Storage
```python
client = chromadb.PersistentClient(path="./chroma_db")
```

## Custom Embedding Model
```python
from chromadb.utils.embedding_functions import OpenAIEmbeddingFunction
embedding_fn = OpenAIEmbeddingFunction(api_key="sk-...", model_name="text-embedding-3-small")
collection = client.create_collection("my_docs", embedding_function=embedding_fn)
```

## Alternatives
| | ChromaDB | Pinecone | pgvector |
|--|---------|----------|----------|
| Setup | pip install | Cloud | PostgreSQL ext |
| Cost | Free | Free tier | Free (self-host) |
| Best for | Prototyping | Production scale | Already using PG |
