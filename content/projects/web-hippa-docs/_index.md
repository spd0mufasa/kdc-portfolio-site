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
