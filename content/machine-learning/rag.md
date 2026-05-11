---
title: Untitled
tags: []
draft: true
date: 2026-05-01
---
retrieval-augmented generation 
- a technique to retrieve external data for LLMs
- RL is a training method -> improve decision-making


2 stages: 
- retrieval:
	- colection of documents 
- generation 
- 


3 core pieces: 
- document processign
- search/retrieval
	- chromadb, faisss, lancedb
- llm for generating answers 
	- ollama run local models like llama3 or mistral 
	- huggingface inference api
	- openrouter 


### ingest
- parsing, cleaning, chunk, converting data into vector embeddings 
- pipeline: unstructured > structured format -> stored in vector database 
- parsing: extracting text from diverse file types + convertinginto a consistent format
- cleaning : normalizing text, removing noise, handling complex formatting
- chunking: breaking large documents -> smaller chunks to ensure that releant context is retrieved rather than just large blocks of text
- embedding generation: ai models to turn text chunks -> numerical vectors (mbeddings) -> represent the semantic meaning of the content 
- indexing & storage: storing embeddings along with metadata in vector database -> fast, retrieval-friendly searching 

#### parsing
- extract, clean, structure raw data -> machine-readable text before embedding 
- 
#### chunking
- splits parsed exts -> manageable segments for embedding 
- typical RAG: chunks by character count 
- 


#### embedding models 
- turns piece of text -> embedding vector of a fixed length that encodes the meaning of that piece of text 

cosine similarity

- recency weigth -> prioritize newer, more relevant info over older, outdated data in ranking systems 
- 

### databases for rag systems:
- elastic search (ES)
	- use cases: relational databases, structured data, logs metrics, monitoring, analyzing logs
- vector databases
	- chromaDB or FAISS: both free 
- knowledge graphs

### generation models
- free: local model using olama like llama 3 8b
- 

 - garbage in garbage out -> quality of rag system depends on quality and structure of source data
 - before vectorizing anything -> clean and deduplicate data 
 - clear scope: what kind of questions the system should sanswer, which internal data sources are best suited to support that 
 - avoid blindly ingesting everything -> less data need to index while still achieving good coverage -> more performant and grounded the system




---
git commit history analyzer that finds similar past bug fixes 

make metadata searchable not just the content
create a read me that shows parsing 