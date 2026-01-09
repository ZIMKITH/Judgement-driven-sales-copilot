Judgment-Driven Sales Copilot

Project Overview
This project is a proof-of-concept for an internal Sales AI tool. The goal was to build a system that can ingest messy, unstructured inputs (like raw Slack dumps or CRM notes) and generate high-stakes outreach drafts.

Unlike standard chatbots, the priority here is restraint. The system is architected to prioritize accuracy over creativity, ensuring it never hallucinates facts not present in the source text.

Architecture Decisions

I intentionally avoided using orchestration frameworks like LangChain for this prototype. I wanted to handle the ingestion logic and state management in pure Python to ensure full visibility into how the data is cleaned and passed to the LLM.

1. Ingestion Pipeline
Real-world sales data is noisy. I wrote a custom cleaning function (using Regex) to sanitize the inputs before embedding them.
Strips internal Slack User ID.
Redacts PII (email addresses) to ensure privacy compliance before data hits the vector store.

2. Retrieval & Storage
   Database: Pinecone (Serverless)
   Embeddings: OpenAI `text-embedding-3-small.
   Strategy: I used dense vector retrieval. For a production version, I would likely add a hybrid search (keyword + semantic) to catch specific account names more reliably.

The Judgment Layer (RAG)
To prevent the "hallucination" common in sales bots, I implemented strict system prompting and set the model temperature to 0.
If the vector search returns low-confidence matches, the model is instructed to admit it doesn't know the answer rather than guessing.
Output styles are hard-coded in the system prompt to match a professional, concise executive tone.

Tech Stack
Python 3
OpenAI API (GPT-4o)
Pinecone Client
Google Colab
