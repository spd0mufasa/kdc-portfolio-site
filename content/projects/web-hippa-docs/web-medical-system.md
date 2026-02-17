---
title: "Web Medical System (UI/GUI)"
description: "Future user interface for interacting with the AI medical document system."
---

# Web Medical System (UI/GUI)

This page describes the planned user interface that will sit on top of the API and ingestion engine. The GUI is intentionally built **last**, after the backend architecture and APIs are stable.

---

## 1. Goals of the Web UI

The web interface will allow each user to:

- Create an account (or be provisioned)
- Upload medical documents
- View their ingested documents
- Ask questions about their own data via agents
- Maintain strict isolation so that:
  - You see only your data  
  - Your sibling sees only her data  
  - Your friend sees only her data  

This mirrors the privacy model of a personal “GitLab for medical records.”

---

## 2. Relationship to the API Layer

The UI is a **thin client** that relies entirely on backend APIs:

- Uses the **Ingestion API** to upload documents  
- Uses the **Retrieval API** to ask questions  
- Uses **User Management APIs** for account actions  

No business logic lives in the UI.  
All intelligence, processing, and retrieval happens in the backend.

---

## 3. Example User Flows

### 3.1 Uploading a Document

1. User logs in  
2. User uploads a PDF or image  
3. UI calls:

