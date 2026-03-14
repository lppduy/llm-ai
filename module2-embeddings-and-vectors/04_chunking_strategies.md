# Chunking Strategies

## Why It Matters
Too big → blurry results. Too small → loses context. Sweet spot: 200-500 words.

## 3 Strategies

### 1. Fixed-size
Split every N characters with overlap. Simple but can cut mid-sentence.

### 2. Recursive (recommended)
Split by structure: paragraphs → lines → sentences → words.
```
Try "\n\n" first → if too big, try "\n" → then ". "
```
LangChain's `RecursiveCharacterTextSplitter` does this.

### 3. Semantic
Uses embeddings to detect topic changes. Best quality, most complex.

## Overlap
```
Without: "...containers to isolate" | "applications. The networking..."
With:    "...containers to isolate applications." | "to isolate applications. The networking..."
```
Preserves context at boundaries. Use 10-20% of chunk size.

## Practical Rule
Start with recursive, 500 chars, 50 overlap. Only switch to semantic if retrieval quality is bad.
