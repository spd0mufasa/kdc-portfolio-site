---
title: "AI Ingestion & Retrieval Engine"
description: "Backend architecture, ingestion pipeline, vector DB, services, containers, and multi-environment design."
---

# AI Ingestion & Retrieval Engine

This page describes the core of the system: how medical documents are ingested, processed, stored, and retrieved using a vector database and a multi-service architecture.

---

## 1. High-Level Architecture Overview

The system ingests medical data from multiple sources, processes it, and stores it in a Vector Database for semantic search by agents.

```text
[Email] ----\
[Scanner] ----\
[Uploads] ------>  [PROCESSING INBOX]  -->  [Pipeline Workers]  -->  [Vector DB]
[APIs] -------/
[Manual Drop]-/
