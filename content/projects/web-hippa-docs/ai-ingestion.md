---
title: "AI Ingestion & Retrieval Engine"
description: "Backend architecture, ingestion pipeline, vector DB, services, containers, and multi-environment design."
---

# AI Ingestion & Retrieval Engine

This page describes the core of the system: how medical documents are ingested, processed, stored, and retrieved using a vector database and a multi-service architecture.

---

## 1. High-Level Architecture Overview

The system ingests medical data from multiple sources, processes it, and stores it in a Vector Database for semantic search by agents.



Absolutely — I can generate **fully‑formed Hugo‑ready `.md` files** for each of the four files you listed.  
You already created the empty files:

```
_index.md
web-medical-system.md
api-ingestion.md
ai-ingestion.md
```

Below are **complete Markdown blocks** you can paste directly into each file.

I’ll format them cleanly so you can copy/paste without editing.

---

# ✅ `_index.md`  
**(PAGE 1 — the hub page with links to the 3 sub‑pages)**

```markdown
---
title: "AI System Project"
description: "A multi-user, HIPAA-aware medical document ingestion and retrieval system."
---

# AI System Project

This project is a backend-first, architecture-heavy system designed to ingest, process, and retrieve medical documents securely for multiple users. It functions like a personal “GitLab for medical records,” where each user has their own private space and agents can query their data through secure APIs.

## Explore the Project

### 1. [Web Medical System (UI/GUI)](./web-medical-system/)
The user interface layer — built last. This will allow users to upload documents, view their data, and interact with agents in a browser.

### 2. [API Ingestion Layer](./api-ingestion/)
The API endpoints that support user management, ingestion, retrieval, and agent access. This is the contract between the backend system and any external clients or agents.

### 3. [AI Ingestion & Retrieval Engine](./ai-ingestion/)
The core engineering work: architecture, ingestion pipeline, vector database, services, containers, metadata schema, and multi-environment design.
```

---

# ✅ `ai-ingestion.md`  
**(Technical Deep Dive — the most important page)**

```markdown
---
title: "AI Ingestion & Retrieval Engine"
description: "Backend architecture, ingestion pipeline, vector DB, services, containers, and multi-environment design."
---

# AI Ingestion & Retrieval Engine

This page describes the core of the system: how medical documents are ingested, processed, stored, and retrieved using a vector database and a multi-service architecture.

---

## 1. High-Level Architecture Overview

The system ingests medical data from multiple sources, processes it, and stores it in a Vector Database for semantic search by agents.

```
[Email] ----\
[Scanner] ----\
[Uploads] ------>  [PROCESSING INBOX]  -->  [Pipeline Workers]  -->  [Vector DB]
[APIs] -------/
[Manual Drop]-/
```

**Key components:**

- Intake channels  
- Processing Inbox  
- Pipeline Workers  
- OCR service  
- Text extraction  
- Embedding generation  
- Vector DB  
- Retrieval API  
- Agents  

---

## 2. Multi-Environment Design (DEV / TEST / STAGE / PROD)

Each environment has:

- Its own Processing Inbox  
- Its own Pipeline Workers  
- Its own Vector DB  
- Its own storage  
- Its own logs  
- Its own credentials  

Only **PROD** contains real medical data.

```
DEV   → synthetic data
TEST  → scrubbed data
STAGE → masked data
PROD  → real UT Health data
```

---

## 3. Docker Architecture (One Service = One Container)

```
+-----------------------------------------------------------+
|                       DOCKER HOST                         |
+-----------------------------------------------------------+
|  Vector DB container                                      |
|  Pipeline Worker container(s)                             |
|  OCR container                                            |
|  Text Extractor container                                 |
|  Embedding container                                      |
|  Retrieval API container                                  |
|  API Gateway container                                    |
|  Logging container                                        |
|  Queue container (optional)                               |
+-----------------------------------------------------------+
```

Each service is isolated but connected via Docker networking and shared volumes.

---

## 4. Directory Structure (Environment-Aware)

```
/project-root/
    /docker/
    /services/
    /data/
        /dev/
        /test/
        /stage/
        /prod/
    /configs/
    /scripts/
```

Each environment has its own raw intake, processing, vector DB, logs, and config directories.

---

## 5. Metadata Schema for the Vector DB

```json
{
  "id": "unique-chunk-id",
  "text": "the extracted text chunk",
  "vector": [...],
  "metadata": {
    "user_id": "U123",
    "source": "UT Health",
    "file_name": "visit_notes_2024_03_12.pdf",
    "file_type": "pdf",
    "file_path": "/data/prod/raw-intake/uploads/visit_notes_2024_03_12.pdf",
    "page_number": 3,
    "chunk_number": 1,
    "chunk_size": 512,
    "ingested_at": "2025-02-14T10:22:00Z",
    "document_type": "clinical_note",
    "tags": ["lab", "visit", "diagnosis"]
  }
}
```

---

## 6. Ingestion Pipeline (Step-by-Step)

```
STEP 1 — File arrives
STEP 2 — Raw storage
STEP 3 — Pre-processing
STEP 4 — Chunking
STEP 5 — Embedding generation
STEP 6 — Vector DB upsert
STEP 7 — Logging & audit
STEP 8 — Ready for search
```

Every step is tied to a specific `user_id`.

---

## 7. Agent Interaction Diagram

```
Agent → Retrieval API → Vector DB → Results
```

Agents never touch raw files directly.

---

## 8. Retrieval API Responsibilities

- Semantic search  
- Metadata filtering  
- Document reconstruction  
- Authentication  
- Authorization  
- Logging  
- Rate limiting  

---

## 9. Key Architectural Principles

- One service = one container  
- One environment = one isolated stack  
- Vector DB stores embeddings + metadata  
- Raw files live in environment-specific storage  
- Retrieval API is the single entry point  
- Every piece of data is tied to a specific `user_id`  
```

---

# ✅ `api-ingestion.md`  
**(API layer overview)**

```markdown
---
title: "API Ingestion Layer"
description: "APIs for user management, ingestion, retrieval, and agent access."
---

# API Ingestion Layer

This page describes the API surface that sits on top of the ingestion and retrieval engine.

---

## 1. Goals of the API Layer

- Multi-user support  
- Secure ingestion  
- Controlled retrieval  
- Auditability  

---

## 2. Core API Domains

### 2.1 User Management

- Create user  
- Update user  
- Deactivate user  
- Delete user (and their data)  

Example user object:

```json
{
  "id": "U123",
  "name": "Your Name",
  "email": "you@example.com",
  "status": "active"
}
```

---

### 2.2 Ingestion API

- `POST /users/{user_id}/documents`  
- `GET /users/{user_id}/documents/{document_id}/status`

---

### 2.3 Retrieval API

Example request:

```json
{
  "query": "Show me my last cholesterol results.",
  "top_k": 5
}
```

The backend filters by `user_id` and performs vector search.

---

### 2.4 Agent Access

Agents interact only through the Retrieval API.

---

## 3. Authentication & Authorization

- API keys or tokens  
- Per-user scoping  
- Role-based access  

---

## 4. Logging & Audit

Every API call logs:

- user_id  
- endpoint  
- timestamp  
- client_id  
- status  

---

## 5. Relationship to the Ingestion Engine

The API layer:

- Validates requests  
- Enqueues work  
- Exposes status  
- Enforces security  
```

---

# ✅ `web-medical-system.md`  
**(GUI — future layer)**

```markdown
---
title: "Web Medical System (UI/GUI)"
description: "Future user interface for interacting with the AI medical document system."
---

# Web Medical System (UI/GUI)

This page describes the planned user interface that will sit on top of the API and ingestion engine.

---

## 1. Goals of the Web UI

- Allow each user to upload documents  
- View their ingested data  
- Ask questions via agents  
- Maintain strict user isolation  

---

## 2. Relationship to the API Layer

The UI is a thin client:

- Uses Ingestion API  
- Uses Retrieval API  
- Uses User Management API  

---

## 3. Example User Flows

### Uploading a Document

1. User logs in  
2. Uploads a PDF  
3. UI calls ingestion API  
4. Shows processing status  

### Asking a Question

1. User types a question  
2. UI calls Retrieval API  
3. Displays agent answer  

---

## 4. Future UI Ideas

- Timeline view  
- Tag-based browsing  
- “Explain this document” mode  
- Lab trend comparisons  

---

## 5. Why the UI Comes Last

The real complexity lives in:

- Ingestion pipeline  
- Vector DB  
- Multi-user isolation  
- Retrieval API  

The UI simply calls the backend.
```

---

If you want, I can also generate:

- A sidebar partial  
- A Hugo menu configuration  
- A diagram-rich version of PAGE 1  
- A README for the whole project  

Just tell me what you want next.

