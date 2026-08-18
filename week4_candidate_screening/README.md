# Day 24 — Company Knowledge AI Assistant

## Overview

This project is an n8n-based RAG (Retrieval-Augmented Generation) assistant for answering questions from a company knowledge base.

It uses Google Gemini for chat generation and text embeddings, and Supabase as the vector database.

The workflow also includes conversation memory and an “information not available” fallback when the knowledge base does not contain enough information.

## Workflow Structure

### 1. Knowledge Ingestion

Start Knowledge Ingestion
→ Create Company Knowledge Entries
→ Default Data Loader
→ Supabase Vector Store

Supporting connections:
- Recursive Character Text Splitter → Default Data Loader
- Embeddings Google Gemini → Supabase Vector Store

The knowledge source contains 24 company knowledge entries, satisfying the assignment requirement of at least 20 entries.

### 2. Company Knowledge Chat

When Chat Message Received
→ AI Agent
→ Respond to Chat

The AI Agent is connected to:
- Google Gemini Chat Model
- Simple Memory
- Company Knowledge Base Search

The knowledge-base tool uses the Supabase vector database with Gemini embeddings to retrieve relevant company information before generating an answer.

## Main Features

- RAG / vector search
- AI-generated answers
- 24 company knowledge entries
- Supabase vector database
- Google Gemini embeddings
- Conversation memory
- Source-aware answers
- “Information not available in the company knowledge base” fallback
- n8n chat interface

## Setup

1. Import the corrected n8n workflow JSON.
2. Select/configure your Google Gemini credentials.
3. Select/configure your Supabase credentials.
4. Make sure the Supabase `documents` table/vector setup is available.
5. Execute the `Start Knowledge Ingestion` branch once to load the company knowledge.
6. Open the n8n chat and test the assistant.

## Example Questions

- How many annual leave days do employees receive?
- Can employees work remotely?
- When are salaries paid?
- What is the training budget?
- How much notice is required for resignation?

Also test a question that is not covered by the knowledge base. The assistant should say that the information is not available instead of inventing an answer.

## Files

- `Day24_Complete_Company_Knowledge_AI_Assistant_CORRECTED.json` — importable n8n workflow
- `company_knowledge.json` — company knowledge source
- `Day24_Complete_Company_Knowledge_AI_Assistant_DESCRIPTION.txt` — node-by-node workflow description
- `README.md` — project overview and setup instructions

## Notes

The ingestion and chat branches are intentionally separate inside the same n8n workflow. Run ingestion first so the vector database contains the company knowledge before testing the chat assistant.
