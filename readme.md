# Advanced RAG on Hugging Face documentation using LangChain
An RAG app that built in top of open source model using HuggingFace.

## Notebook Goal

This notebook is for learning purpuse of how to impliment RAG apps Using LangChain

# 1. Retriever - embeddings 🗂️
The retriever acts like an internal search engine: given the user query, it returns a few relevant snippets from your knowledge base.

These snippets will then be fed to the Reader Model to help it generate its answer.

## 1.1 Text Splitter

Text splitter aims to split thr row docs into smaller chunks of text that could be emmbedded without lossing or truncate tokens

### RecursiveCharacterTextSplitter with tokenizer

By defult RecursiveCharacterTextSplitters split the docs based on character count. It is the basic count function.

**Tokenizer** plays the role of the count function that allows us to split by token

## 1.2 Building the vector database

Building the vector database based on FAISS liberary.

It semply maps the chunked text into 1-D vector.

# 2. Reader - LLM

In this part, the LLM Reader reads the retrieved context to formulate its answer.

There are substeps that can all be tuned:

1. The content of the retrieved documents is aggregated together into the “context”, with many processing options like prompt compression.
2. The context and the user query are aggregated into a prompt and then given to the LLM to generate its answer.


### 2.1. Reader model
The choice of a reader model is important in a few aspects:

- the reader model’s max_seq_length must accommodate our prompt, which includes the context output by the retriever call: the context consists of 5 documents of 512 tokens each, so we aim for a context length of 4k tokens at least.
- the reader model

To make inference faster, we will load the quantized version of the model:

### 2.2. Prompt
The RAG prompt template below is what we will feed to the Reader LLM: it is important to have it formatted in the Reader LLM’s chat template.

We give it our context and the user’s question.


### 2.3. Reranking
A good option for RAG is to retrieve more documents than you want in the end, then rerank the results with a more powerful retrieval model before keeping only the top_k.

For this, Colbertv2 is a great choice: instead of a bi-encoder like our classical embedding models, it is a cross-encoder that computes more fine-grained interactions between the query tokens and each document’s tokens.

