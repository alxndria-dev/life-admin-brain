# Life Admin Brain

A local RAG (retrieval-augmented generation) assistant for your personal documents.

Drop your lease, insurance, warranties, and passport into a folder. Then ask "when does my passport expire?" or "what does my lease say about breaking early?" and get an answer with the source cited. Everything stays on your machine.

## How it works

```
docs/*.pdf, *.txt, *.md
        ↓  ingest.py
   split into ~800-char chunks
        ↓
   embed each chunk into a vector (local)
        ↓
   index.json  (your local vector store)

question ──embed──▶ cosine similarity ──▶ top 4 chunks ──▶ answer
                    (all local)                          (Claude, optional)
```

## How to use it

Ingest your files:

```py
python ingest.py
```

Ask your query:
```py
python ask.py "when does my passport expire?"
```

## Prerequisites

- Python 3.10+
- Optional: an Anthropic API key for natural-language answers
- Optional: pip install sentence-transformers for real semantic embeddings
- Optional: pip install pypdf to read PDF files (text/markdown work with no installs)

## Common pitfalls

- Chunks too big: one 5-page chunk retrieves everything and answers nothing. - Keep chunks small enough that one covers a single idea.
- Forgot to reingest: edited a doc but answers are stale? Rerun `ingest.py`.
- Expecting magic from the hashing fallback: it matches words, not meaning. Install `sentence-transformers` before judging retrieval quality.
- Sending whole documents to Claude: don't. The point of RAG is you only send the few chunks that matter, which keeps it cheap and private.