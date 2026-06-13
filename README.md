# AI-PDF-Chatbot-using-LangChain-and-LangGraph
ingests ingests PDFs, generates embeddings, and performs retrieval-augmented generation using LangChain and LangGraph.

**Here's what the Chatbot UI looks like:**


<img width="1096" alt="Screenshot 2025-02-20 at 05 39 55" src="https://github.com/user-attachments/assets/3a9ddea7-b718-476b-bdae-38839be20c12" />

## Features

- **Document Ingestion Graph**: Upload and parse PDFs into `Document` objects, then store vector embeddings into a vector database i used supabase.
- **Retrieval Graph**: Handle user questions, decide whether to retrieve documents or give a direct answer, then generate concise responses with references to the retrieved documents.
- **Streaming Responses**: Real-time streaming of partial responses from the server to the client UI.
- **LangGraph Integration**: Built using LangGraph’s state machine approach to orchestrate ingestion and retrieval, visualise your agentic workflow, and debug each step of the graph.  
- **Next.js Frontend**: Allows file uploads, real-time chat, and easy extension with React components and Tailwind.

- ## Architecture Overview

```ascii
┌─────────────────────┐    1. Upload PDFs    ┌───────────────────────────┐
│Frontend (Next.js)   │ ────────────────────> │Backend (LangGraph)       │
│ - React UI w/ chat  │                      │ - Ingestion Graph         │
│ - Upload .pdf files │ <────────────────────┤   + Vector embedding via  │
└─────────────────────┘    2. Confirmation   │     SupabaseVectorStore   │
(storing embeddings in DB)

┌─────────────────────┐    3. Ask questions  ┌───────────────────────────┐
│Frontend (Next.js)   │ ────────────────────> │Backend (LangGraph)       │
│ - Chat + SSE stream │                      │ - Retrieval Graph         │
│ - Display sources   │ <────────────────────┤   + Chat model (OpenAI)   │
└─────────────────────┘ 4. Streamed answers  └───────────────────────────┘

```
- **Supabase** is used as the vector store to store and retrieve relevant documents at query time.  
- **OpenAI** (or other LLM providers) is used for language modeling.  
- **LangGraph** orchestrates the "graph" steps for ingestion, routing, and generating responses.  
- **Next.js** (React) powers the user interface for uploading PDFs and real-time chat.
- The system consists of:
- **Backend**: A Node.js/TypeScript service that contains LangGraph agent "graphs" for:
  - **Ingestion** (`src/ingestion_graph.ts`) - handles indexing/ingesting documents
  - **Retrieval** (`src/retrieval_graph.ts`) - question-answering over the ingested documents
  - **Configuration** (`src/shared/configuration.ts`) - handles configuration for the backend api including model providers and vector stores
- **Frontend**: A Next.js/React app that provides a web UI for users to upload PDFs and chat with the AI.
---
