---
title: "API Ingestion Layer"
description: "APIs for user management, ingestion, retrieval, and agent access."
---

# API Ingestion Layer

This page describes the API surface that sits on top of the ingestion and retrieval engine. It is the contract between the backend system and any external clients or agents.

The API layer is responsible for:
- Creating and managing users  
- Accepting documents for ingestion  
- Allowing agents to query a user’s data  
- Enforcing strict user isolation  
- Logging and auditing all actions  

---

## 1. Goals of the API Layer

The API layer exists to provide a secure, structured interface into the ingestion and retrieval engine. Its goals include:

- **Multi-user support:** Each user has their own private medical data.
- **Secure ingestion:** Users (or trusted clients) can upload documents via APIs.
- **Controlled retrieval:** Agents can query data only for a specific user.
- **Auditability:** All actions are logged for traceability and compliance.
- **HIPAA-aware design:** No cross-user access, strict scoping, and full logging.

---

## 2. Core API Domains

The API layer is divided into four major domains.

---

### 2.1 User Management API

These endpoints manage user accounts. Even if the UI is built later, the backend must support:

- **Create user**
- **Update user**
- **Deactivate user**
- **Delete user** (and all associated data)

Example user object:

```json
{
  "id": "U123",
  "name": "Your Name",
  "email": "you@example.com",
  "created_at": "2025-02-17T10:00:00Z",
  "status": "active"
}
