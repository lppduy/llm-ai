# Embedding Models (Local vs Cloud)

## Comparison

| Model | Provider | Dims | Cost | Quality |
|-------|----------|------|------|---------|
| text-embedding-3-small | OpenAI | 1536 | $0.02/1M tokens | Good |
| text-embedding-3-large | OpenAI | 3072 | $0.13/1M tokens | Best |
| nomic-embed-text | Ollama | 768 | Free | Good enough |
| mxbai-embed-large | Ollama | 1024 | Free | Better |

## Which to Use
- Learning/prototyping → nomic-embed-text (free, local)
- Production on budget → text-embedding-3-small
- Best quality → text-embedding-3-large

## Critical Rule
**Never mix models.** Embed docs with model A → must query with model A.
Switching models = re-embed everything.
