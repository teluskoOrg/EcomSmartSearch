# 🛒 E-Commerce Product Management System with Smart Semantic Search

## 📘 Executive Summary

The **E-Commerce Product Management System** is a high-performance backend solution developed using **Spring Boot 3.5.3** and **Java 21**, designed to support core e-commerce operations. This version focuses on smart product search using **Spring AI** and **PGVector** for semantic search capabilities. It excludes chatbot functionality and AI-generated product descriptions/images to keep the scope lightweight and focused on intelligent search and product management.

---

## 📑 Table of Contents

* [Project Objective](#project-objective)
* [Core Features](#core-features)
* [Technologies Used](#technologies-used)
* [Dependencies](#dependencies)
* [System Architecture](#system-architecture)
* [API Specifications](#api-specifications)
* [Semantic Search Integration](#semantic-search-integration)
* [PostgreSQL Extension Setup](#postgresql-extension-setup)
* [Installation Guide](#installation-guide)
* [Folder Structure (Backend)](#folder-structure-backend)

---

## 🎯 Project Objective

To build a scalable, efficient backend that supports CRUD operations for products and provides **smart semantic product search** using **RAG (Retrieval-Augmented Generation)** powered by PGVector and OpenAI.

---

## 🔧 Core Features

* Add, update, delete, and retrieve products
* Product image upload (manual only)
* Smart product search using natural language queries and vector search

---

## 💡 Technologies Used

| Component       | Technology                     |
| --------------- | ------------------------------ |
| Backend         | Spring Boot 3.5.3              |
| ORM             | Hibernate, Spring Data JPA     |
| Database        | PostgreSQL + PGVector (Docker) |
| Semantic Search | Spring AI + GPT-4o + Embedding |
| Containerization | Docker & Docker Compose       |
| Runtime         | Java 21                        |
| Build Tool      | Apache Maven                   |

---

## 📦 Dependencies

| Group ID                 | Artifact                                | Scope    |
| ------------------------ | --------------------------------------- | -------- |
| org.springframework.boot | spring-boot-starter-web                 | compile  |
| org.springframework.boot | spring-boot-starter-data-jpa            | compile  |
| org.springframework.boot | spring-boot-docker-compose              | runtime  |
| org.postgresql           | postgresql                              | runtime  |
| org.projectlombok        | lombok                                  | optional |
| org.springframework.ai   | spring-ai-starter-model-openai          | compile  |
| org.springframework.ai   | spring-ai-starter-vector-store-pgvector | compile  |

---

## 🧱 System Architecture

* **Controller Layer**: Exposes product-related API endpoints
* **Service Layer**: Business logic including semantic search integration
* **Repository Layer**: Spring Data JPA for persistence
* **Vector Layer**: PGVector-based semantic search integration

---

## 🔗 API Specifications

| Endpoint                               | Method | Description                           |
| -------------------------------------- | ------ | ------------------------------------- |
| `/api/products`                        | GET    | Get all products                      |
| `/api/product/{id}`                    | GET    | Get product by ID                     |
| `/api/product`                         | POST   | Add new product with image upload     |
| `/api/product/{id}`                    | PUT    | Update existing product               |
| `/api/product/{id}`                    | DELETE | Delete product by ID                  |
| `/api/products/smart-search?query=xyz` | GET    | Semantic search for matching products |

---

## 🔍 Semantic Search Integration

The smart search feature uses **Spring AI** and **PGVector** to allow natural language queries to fetch relevant products. The system:

* Converts product data into semantic vectors and stores them in PGVector
* Uses a prompt-based `product-search-prompt.st` file
* Calls OpenAI GPT-4o to generate a filtered product list
* Converts the JSON output into actual product entities by IDs

Prompt logic and context fetching is handled inside the `ProductService`.

---

## 🐳 Docker Configuration

### docker-compose.yml

```yaml
services:
  pgvector:
    image: 'pgvector/pgvector:pg16'
    environment:
      - 'POSTGRES_DB=telusko'
      - 'POSTGRES_PASSWORD=0076'
      - 'POSTGRES_USER=postgres'
    labels:
      - "org.springframework.boot.service-connection=postgres"
    ports:
      - '5432'
```

---

## 🧩 PostgreSQL Extension Setup (PGVector)

Ensure the following extensions and schema are initialized:

```sql
CREATE EXTENSION IF NOT EXISTS vector;
CREATE EXTENSION IF NOT EXISTS hstore;

CREATE TABLE IF NOT EXISTS vector_store (
    id TEXT PRIMARY KEY,
    content TEXT,
    metadata JSONB,
    embedding VECTOR(1536)
);

CREATE INDEX IF NOT EXISTS vector_store_embedding_idx ON vector_store USING HNSW (embedding vector_cosine_ops);
```

Spring Boot properties:

```properties
spring.sql.init.schema-locations=classpath:init/schema.sql
spring.sql.init.mode=always
spring.ai.openai.embedding.options.model=text-embedding-ada-002
```

---

## 🛠 Installation Guide

### Prerequisites
- Java 21 installed
- Docker Desktop running
- IntelliJ IDEA or any IDE

### 🚀 Quick Start

**Step 1:** Open the project in IntelliJ IDEA or your preferred IDE

**Step 2:** Ensure Docker Desktop is running

**Step 3:** Right-click on the root project directory and open terminal

**Step 4:** Start the PostgreSQL database with PGVector extension
```bash
docker-compose up -d
```

**Step 5:** Run the Spring Boot application
- Navigate to `SpringEcomApplication.java`
- Click the Run button or use `Ctrl+Shift+F10`

🎉 **Your AI-Powered E-Commerce Backend with Smart Semantic Search is now running!**

---

## 📂 Folder Structure (Backend)

```
com.telusko.springecom
├── config               # VectorStore, ChatClient config
├── controller           # REST APIs for Product
├── model                # Product entity
├── repo                 # ProductRepo for DB operations
├── service              # Semantic search logic and image upload
├── prompts              # Prompt template for smart search
└── SpringEcomApplication.java
```

---
## 👨‍💻 Author

Developed and maintained by **Telusko Team**.

---

> ⚙️ **Scalable. Intelligent. AI-Powered.** Ready for the next-gen E-Commerce experience.