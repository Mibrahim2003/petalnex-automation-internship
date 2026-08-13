# Day 21 — Nexora HR Policy RAG Assistant

## Files

- `Day 21 - HR Policy Knowledge Base Ingestion.json`
- `Day 21 - Nexora HR Policy Assistant.json`

## Architecture

Workflow 1:
Google Drive folder -> Download PDFs -> Extract PDF text -> Clean + source metadata -> Default Data Loader -> Recursive Character Text Splitter -> Gemini Embeddings -> Supabase Vector Store

Workflow 2:
Chat Trigger -> Question and Answer Chain -> Vector Store Retriever -> Supabase Vector Store + Gemini Embeddings -> Gemini Chat Model

The workflows do not need a direct node-to-node connection. They share the same Supabase `documents` table. Workflow 1 populates it; Workflow 2 retrieves from it.

## Required setup after importing Workflow 1

1. Open `CONFIG - Google Drive Folder ID`.
2. Replace `REPLACE_WITH_GOOGLE_DRIVE_FOLDER_ID` with the Google Drive folder ID that contains the HR policy PDFs.
3. Confirm the Google Drive credential.
4. Confirm the Supabase credential, `documents` table, and `match_documents` RPC/function.
5. Confirm the Gemini credentials and, most importantly, that the embedding dimension matches the existing Supabase vector column from Day 20.
6. Run Workflow 1 once.

## Required setup after importing Workflow 2

1. Confirm the Supabase credential.
2. Confirm the same `documents` table and `match_documents` RPC/function.
3. Confirm the same Gemini embedding credential/model family used by Workflow 1.
4. Confirm the Gemini Chat Model credential.
5. Open the chat UI and test the examples below.

## Test questions

1. How many casual leaves are allowed?
2. What are the standard working hours?
3. What are the attendance rules?
4. What are the internship requirements?
5. What is the maternity leave policy?  <-- this should trigger the fallback if maternity leave is not in the knowledge base.

## Fallback

The Q&A system prompt requires this exact response when the retrieved context does not support the answer:

`Information not available in the knowledge base.`

## Important

This package uses Google Drive, not Read/Write Files from Disk, because the workflow is intended for n8n Cloud.
