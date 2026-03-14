# RAG with LangChain

## Setup
```bash
pip install langchain langchain-openai langchain-chroma
```

## Code
```python
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
from langchain_chroma import Chroma
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.document_loaders import DirectoryLoader

# Load → Chunk → Embed → Store
docs = DirectoryLoader("./my_docs", glob="**/*.txt").load()
chunks = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50).split_documents(docs)
vectorstore = Chroma.from_documents(chunks, OpenAIEmbeddings())

# Query
results = vectorstore.as_retriever(search_kwargs={"k": 3}).invoke("refund policy?")
context = "\n".join([doc.page_content for doc in results])
response = ChatOpenAI(model="gpt-4o-mini").invoke(f"Context:\n{context}\n\nQuestion: refund policy?")
```

## LlamaIndex vs LangChain
| | LlamaIndex | LangChain |
|--|-----------|-----------|
| Focus | RAG-first | General LLM toolkit |
| RAG setup | 5 lines | 15+ lines |
| Best for | "Just need RAG" | RAG + agents + tools |

Start with LlamaIndex for RAG. LangChain when you need agents.
