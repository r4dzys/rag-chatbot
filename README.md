---
title: RAG Document Q&A
emoji: 📚
colorFrom: blue
colorTo: indigo
sdk: docker
pinned: false
---

# RAG Document Q&A System

A production-ready Retrieval Augmented Generation (RAG) application that allows users to upload documents and ask questions based on their content.

## Live Demo
[huggingface.co/spaces/rado-zys/rag-document-qa](https://huggingface.co/spaces/rado-zys/rag-document-qa)

## Features
- Upload multiple documents simultaneously (PDF, TXT, DOCX)
- RAG pipeline with semantic search across uploaded documents
- Conversation history — model remembers previous questions
- Context transparency — see exactly which document fragments were used to generate each answer
- New Conversation button — reset session and load new documents instantly

## Architecture

### System Architecture

```mermaid
graph TB
    User["👤 User"]
    UI["🖥️ Streamlit UI<br/>Hugging Face Spaces"]
    Upload["📁 File Upload<br/>PDF / TXT / DOCX"]
    Loader["📄 Document Loader<br/>LangChain"]
    Splitter["✂️ Text Splitter<br/>chunk_size=500, overlap=50"]
    Embeddings["🔢 Embeddings<br/>all-MiniLM-L6-v2"]
    VectorStore["🗄️ Vector Store<br/>ChromaDB"]
    Search["🔍 Similarity Search<br/>Top 3 chunks"]
    LLM["🤖 LLM<br/>Llama 3.3 70B via Groq"]
    Answer["💬 Answer + Context"]

    User --> UI
    UI --> Upload
    Upload --> Loader
    Loader --> Splitter
    Splitter --> Embeddings
    Embeddings --> VectorStore
    User -->|"Question"| Search
    VectorStore --> Search
    Search -->|"Retrieved context"| LLM
    LLM --> Answer
    Answer --> User
```

## Workflow

```mermaid
flowchart TD
    A([Start]) --> B[Upload documents\nPDF / TXT / DOCX]
    B --> C{Valid file type?}
    C -->|No| D[Return error 400]
    C -->|Yes| E[Load document content]
    E --> F[Split into chunks\nchunk_size=500, overlap=50]
    F --> G[Generate embeddings\nall-MiniLM-L6-v2]
    G --> H[Store in ChromaDB\nvector store]
    H --> I[Session ready ✅]
    I --> J[User asks question]
    J --> K[Embed question]
    K --> L[Similarity search\nTop 3 chunks]
    L --> M[Build prompt\ncontext + history + question]
    M --> N[Send to Llama 3.3 70B\nvia Groq API]
    N --> O[Return answer + context]
    O --> P[Save to conversation history]
    P --> Q{Another question?}
    Q -->|Yes| J
    Q -->|No| R[Clear history or\nupload new documents]
    R --> S([End])
```

## Tech Stack
- **LLM:** Llama 3.3 70B via Groq API
- **Embeddings:** sentence-transformers/all-MiniLM-L6-v2
- **Vector Store:** ChromaDB
- **Framework:** LangChain
- **UI:** Streamlit
- **Deployment:** Docker on Hugging Face Spaces

## Local Setup
```bash
git clone https://github.com/r4dzys/ai-architect-journey
cd ai-architect-journey/rag-document-qa
pip install -r requirements.txt
export GROQ_API_KEY=your_key_here
streamlit run src/streamlit_app.py
```

## Project Context
Focus: understanding RAG architecture end-to-end — from document ingestion to production deployment.

## Author
Radosław Zys | [LinkedIn](https://linkedin.com/in/radoslaw-zys) | [GitHub](https://github.com/r4dzys)
