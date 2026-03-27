# da-form-ai-assistant
A no-code AI chatbot - RAG-powered AI chatbot that answers questions from uploaded PDF documents — built with n8n, Pinecone, and Google Gemini. No backend code required.

**Project Overview**

 - DA Form AI Assistant is an owner-controlled, RAG-based chatbot where the academy admin uploads a rules or policy PDF once, and users can then ask unlimited questions through a public chat interface. Users never see, access, or interact with the document directly — they simply ask questions in plain English and receive accurate, document-grounded answers from the AI.
 - The entire pipeline is built visually in n8n, with no custom backend code. It uses Google Gemini for embeddings and language generation, and Pinecone as the vector database for fast semantic retrieval.

**Problem Statement**

  In many academies, rules and admission policies are stored in PDFs that staff and students rarely read in full. Important information gets missed, and the support team spends time answering repetitive questions that are already in the document. There was no automated, always-available assistant that could answer these questions instantly and accurately.

**Solution**

- This project solves the problem by splitting responsibilities clearly:
- The owner uploads the PDF document once — the system processes and indexes it automatically
- Users get a clean chat interface to ask any question at any time
- The AI retrieves the relevant section of the document and answers — no guessing, no hallucinations
- The document stays private; users only receive answers, not the raw content

**How It Works — Two Pipelines**

**Pipeline 1:**

- Owner Document Upload (Private)
- Owner opens the private n8n Form Trigger URL
- Owner uploads the academy rule/policy PDF
- Default Data Loader parses the PDF and extracts all text
- Gemini Embedding-001 converts text chunks into semantic vectors
- Vectors are stored in Pinecone index 'daform' — this runs once and persists

**Pipeline 2:** 

- User Chat Interface (Public)
- User opens the public chat link and types a question
- AI Agent receives the question
- Pinecone is queried for the most relevant document chunks
- Gemini 2.5 Flash Lite generates a clear, grounded answer from those chunks
- The answer is returned to the user — document is never exposed directly

Features
Owner-only document upload — users have zero access to the PDF
Public chat interface — accessible by any user with the link
RAG architecture — answers always based on the actual document content
Conversation memory — AI remembers previous messages in the same session
Semantic search — finds relevant content even if the exact words differ
No-code pipeline — fully built and managed in n8n with no backend server

**Tech Stack**

## Tech Stack Used

| Technology      | Type                     | Role                                                                 |
|----------------|--------------------------|----------------------------------------------------------------------|
| n8n            | Workflow Automation      | Orchestrates both owner ingestion and user chat pipelines            |
| Pinecone       | Vector Database          | Stores and retrieves document embeddings for semantic search         |
| Google Gemini  | LLM + Embedding Model    | Generates embeddings (gemini-embedding-001) and AI responses (gemini-2.5-flash-lite) |
| RAG            | Architecture Pattern     | Grounds chatbot answers in the owner-uploaded rule document          |
| PDF Loader     | Document Parser          | Parses the PDF uploaded by the owner into text chunks                |

**Setup Instructions**

**Prerequisites**

- n8n account (cloud or self-hosted)
- Pinecone account — create an index named 'daform' (dimensions: 768, metric: cosine)
- Google Gemini API key from Google AI Studio

**Import & Configure**

- Download the workflow JSON from this repository
- In n8n: Workflows > Import from File > upload the JSON
- Add credentials: Google Gemini API key and Pinecone API key
- Activate the workflow

**Owner Steps**

- Open the private Form Trigger URL (from n8n)
- Upload the rule/policy PDF
- Wait for the workflow to complete — document is now indexed in Pinecone

**User Steps**

- Open the public Chat interface URL
- Ask any question related to the academy rules
- Receive instant, accurate answers from the AI

**Future Improvements**

- Admin dashboard to manage and replace uploaded documents
- Support for multiple documents with source tagging
- Role-based access with a login system for the owner panel
- WhatsApp or Telegram bot integration for users
- Analytics to track frequently asked questions
- Auto-notification to owner when unanswerable questions are detected

