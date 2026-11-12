---
title: RAG
nav_order: 9
layout: home
permalink: /rag/
---

# Retrieval-Augmented Generation

RAG is the pattern of retrieving relevant material from your own corpus at query time and putting it in the prompt, rather than relying on what the model memorised in training. The original paper is [Lewis et al., 2020](https://arxiv.org/abs/2005.11401).

---

## The pipeline

```
documents -> parse -> chunk -> embed + index -> retrieve based on prompt -> rerank (optional) -> insert into prompt -> answer + citations
```

---

## 1. Parsing

What you get differs by track: plain text manuals (Track 1), legal PDFs (Track 2), CSV records (Track 3). The PDFs are the hard case. We recommend the following two libraries:

- **[PyMuPDF](https://pymupdf.readthedocs.io/)** gives you text with coordinates and font sizes, which is how you detect headings and tables programmatically.
- **[Unstructured](https://github.com/Unstructured-IO/unstructured)** handles many formats with one API and emits typed elements (title, list item, table).

It is worthwhile to do a manual check on a few documents, to see whether the parser is actually giving you the text you expect. 

---

## 2. Chunking

You are cutting documents into retrievable units. The tension: chunks small enough to be precise, large enough to be self-contained. There are various strategies that you can consider:

- **Fixed-size with overlap** (say 500-1000 tokens, 10-20% overlap) is the default and a perfectly reasonable starting point. See the [LangChain text splitter concepts](https://python.langchain.com/docs/concepts/text_splitters/) for the standard variants.
- **Structure-aware splitting** is possible when the documents have headings, sections, or tables. A chunk that is a whole section is more likely to be self-contained than one that is a random slice of text.
- **Prepend the context** to each chunk: the manual name and section heading, or the act and article number. A chunk that begins "KMV Annex 1, KM 5 - ..." retrieves better and cites better than a bare paragraph.
- **Contextual retrieval**: use the model itself to write a one-or-two-sentence situating preamble for each chunk before embedding it. Anthropic's [write-up](https://www.anthropic.com/news/contextual-retrieval) reports a large drop in retrieval failures. However, be aware of the cost when doing so.

For more background reading, have a look at [this article](https://www.trychroma.com/research/evaluating-chunking).

{: .tip }
> Keep an eye on what a retrieved chunk looks like as plain text in the prompt. If you cannot tell, reading it yourself, which document and section it came from, neither can the model.

---

## 3. Embedding and indexing

For embedding, the inference endpoint hosts one model, called on `/embeddings` with this exact id:

| Model id | Dimensions | Notes |
|---|---|---|
| `RCP-AIaaS/Qwen/Qwen3-Embedding-8B` | 4096 | multilingual, strong general-purpose model |

You can also run a small embedding model locally on your VM with [sentence-transformers](https://www.sbert.net/), for example `BAAI/bge-small-en-v1.5` or `intfloat/multilingual-e5-small`. It can be worthwhile to compare retrieval quality between the hosted and a local model on your corpus. The embedding model is **not** the same as the model you use for generation; you can mix and match.

---

## 4. Reranking

When you have a list of candidate documents, you can use a reranker to re-evaluate their relevance to the query. This is particularly useful when you have a large number of candidates and want to improve the quality of the final results.

We can do so by retrieving generously (say 20-50 candidates), then rerank and keep the best handful. A cross-encoder reads the query and the candidate *together*, which is more accurate than comparing two independently computed vectors.

You can use an LLM-as-reranker (ask the model to score each candidate's relevance) with one of the chat models on the endpoint, but be aware of the cost in latency against your response budget. A dedicated cross-encoder such as [`bge-reranker-v2-m3`](https://huggingface.co/BAAI/bge-reranker-v2-m3) runs locally on your VM and is cheaper per candidate.

---

## 5. Generation

- **Number the chunks** and ask for citations by number, then map numbers back to real references in your code. Models are much better at "[3]" than at reproducing a document identifier correctly.
- **Say what to do when the context is insufficient**, explicitly, and give an example. Without that instruction the model will reach for parametric knowledge, and you will be scored on an answer that is not in the corpus.

---

## Frameworks

[LangChain](https://github.com/langchain-ai/langchain), [LlamaIndex](https://docs.llamaindex.ai/) and [Haystack](https://haystack.deepset.ai/) are all powerful frameworks to build a RAG pipeline.

However, there are many more options out there, you can also have a look at [FlashRAG](https://github.com/RUC-NLPIR/FlashRAG) and [this repo](https://github.com/NirDiamant/RAG_Techniques).

For each of these frameworks, there are a lot of doucmentation and examples online.

