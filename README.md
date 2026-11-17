# 🚀 SYNORA Enterprise AI
### Intelligent Enterprise Knowledge Assistant powered by RAG, AWS, and Generative AI

<p align="center">
  <strong>Transform enterprise documents into an intelligent, searchable knowledge system.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/FastAPI-Backend-009688?logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/AWS-Amazon%20Bedrock-FF9900?logo=amazonaws&logoColor=white" />
  <img src="https://img.shields.io/badge/LangGraph-Workflow%20Orchestration-1C3C3C" />
  <img src="https://img.shields.io/badge/Pinecone-Vector%20Database-6C5CE7" />
  <img src="https://img.shields.io/badge/PostgreSQL-Database-4169E1?logo=postgresql&logoColor=white" />
</p>

---

## 🌐 Overview

**SYNORA Enterprise AI** is an AI-powered enterprise document intelligence platform designed to help organizations retrieve information from internal documents and generate context-aware answers using Retrieval-Augmented Generation (RAG).

Organizations maintain large volumes of information across PDFs, policies, technical documentation, reports, and other business documents. Finding relevant information manually can be time-consuming and inefficient.

SYNORA addresses this challenge by combining document processing, semantic search, vector databases, and Large Language Models into an integrated knowledge assistant.

The system uses Amazon S3 for document storage, Amazon Bedrock for embedding generation and response generation, and Pinecone for vector-based retrieval. PostgreSQL supports relational application data, while FastAPI provides the backend API layer.

The application is designed around an enterprise knowledge retrieval workflow, with additional capabilities for document management, access considerations, observability, and workflow governance.

## ✨ Key Features

### 📄 1. Enterprise Document Management
- Supports document upload workflows.
- Uses Amazon S3 for document storage.
- Organizes uploaded documents using tenant-oriented storage paths.
- Supports document ingestion and processing workflows.
- Maintains document-related metadata in PostgreSQL.

### 🧠 2. Retrieval-Augmented Generation (RAG)
- Converts document content into vector embeddings.
- Uses Amazon Titan Text Embeddings V2.
- Stores embeddings in Pinecone.
- Performs semantic similarity-based retrieval.
- Supplies retrieved evidence to the LLM for answer generation.
- Generates context-aware responses grounded in available document information.

### 🤖 3. Amazon Bedrock Integration
Uses Amazon Bedrock to access foundation models:

| Model | Responsibility |
|---|---|
| Amazon Titan Text Embeddings V2 | Generates document and query embeddings. |
| Amazon Nova Lite | Generates natural-language answers using retrieved context. |

### 🔍 4. Semantic Search with Pinecone
- Stores document embeddings in a vector index.
- Retrieves semantically relevant document chunks.
- Supports tenant-based namespaces.
- Uses cosine similarity for vector search.
- Uses 1024-dimensional embeddings.

### 🔐 5. Enterprise-Oriented Data Management
- Uses PostgreSQL for relational application data.
- Maintains document and conversation-related records.
- Includes tenant-oriented document organization.
- Supports audit and approval-related data structures.

### 🛡️ 6. Guardrails and Workflow Governance
The application incorporates guardrail and approval-related components to support controlled enterprise workflows.

These mechanisms help manage how requests and generated responses are handled.

### 📊 7. Observability
Integrates RIUS observability to support application monitoring and execution visibility.

Configuration includes:
- Service identification.
- API-based observability configuration.
- Content-capture controls.
- Observability enable/disable settings.

---

## 🏗️ System Architecture

The following diagram represents the main document ingestion and RAG architecture.

```mermaid
flowchart TD
    U["User"] --> UI["Web Application"]
    UI --> API["FastAPI Backend"]

    API --> UP["Document Upload"]
    UP --> S3["Amazon S3"]

    S3 --> W["Document Ingestion Worker"]
    W --> EX["Document Processing"]
    EX --> CH["Text Chunking"]

    CH --> EMB["Amazon Titan Embeddings V2"]
    EMB --> PC["Pinecone Vector Database"]

    U --> Q["User Question"]
    Q --> API
    API --> QE["Query Embedding"]
    QE --> PC

    PC --> RET["Relevant Document Chunks"]
    RET --> PROMPT["RAG Prompt Construction"]

    Q --> PROMPT
    PROMPT --> NOVA["Amazon Nova Lite"]
    NOVA --> RESP["Generated Answer"]
    RESP --> UI

    API <--> PG[("PostgreSQL")]
    API -.-> RIUS["RIUS Observability"]
```

*The diagram represents the high-level architecture. Actual execution and component interactions are determined by the application implementation.*

---

## 🔄 How the RAG Pipeline Works

SYNORA follows a Retrieval-Augmented Generation workflow.

### Phase 1 — Document Ingestion

**Step 1: Document Upload**

Users upload documents through the application. Amazon S3 provides object storage for uploaded files.

**Step 2: Document Processing**

The ingestion worker processes documents retrieved from S3.

**Step 3: Text Extraction and Chunking**

Document content is prepared and divided into smaller text chunks for embedding generation and retrieval.

**Step 4: Embedding Generation**

Amazon Titan Text Embeddings V2 converts the document chunks into numerical vector representations.

**Step 5: Vector Storage**

The generated embeddings and associated metadata are stored in Pinecone.

The configured embedding dimension is 1024, and the index uses cosine similarity.

### Phase 2 — Question Answering

**Step 1: User Question**

The user submits a natural-language question through the web application.

**Step 2: Query Embedding**

The question is converted into a vector using the same embedding model used for document indexing.

**Step 3: Semantic Retrieval**

Pinecone searches for relevant document chunks based on vector similarity.

**Step 4: Context Construction**

The application combines the retrieved evidence with the user's question and relevant conversation history.

**Step 5: Answer Generation**

Amazon Nova Lite receives the constructed prompt through Amazon Bedrock and generates a natural-language response.

**Step 6: Response Delivery**

The generated answer is returned to the frontend.

### Important RAG Concept

Pinecone does not generate answers.

Its responsibility is to retrieve relevant information from stored document embeddings.

Amazon Nova Lite is the generative model responsible for understanding the user's question and retrieved context and producing the final response.

---

## 🧩 Technology Stack

| Category | Technologies |
|---|---|
| Programming Language | Python 3.11 |
| Backend Framework | FastAPI |
| AI Orchestration | LangGraph |
| LLM Framework | LangChain |
| Generative AI | Amazon Nova Lite |
| Embedding Model | Amazon Titan Text Embeddings V2 |
| Model Platform | Amazon Bedrock |
| Object Storage | Amazon S3 |
| Vector Database | Pinecone |
| Relational Database | PostgreSQL |
| Observability | RIUS |
| Frontend | HTML, CSS, JavaScript |
| Deployment Infrastructure | Docker, AWS services |

---

## 📁 Project Structure

```text
SYNORA-Enterprise-AI/
│
├── app/
│   ├── main.py
│   │   └── FastAPI application and API endpoints
│   │
│   ├── db.py
│   │   └── PostgreSQL connection and database operations
│   │
│   ├── vector.py
│   │   └── Pinecone index management and vector operations
│   │
│   └── ...
│
├── worker/
│   └── ingest.py
│       └── Document ingestion and processing workflow
│
├── static/
│   └── js/
│       └── app.js
│
├── templates/
│
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── .gitignore
└── README.md
```

*The structure above highlights the application files identified during debugging. Add any additional directories from your final repository.*

---

## ⚙️ Installation and Setup

### Prerequisites

Make sure you have:

- Python 3.11
- Git
- Conda or Python virtual environment
- AWS account with Amazon Bedrock access
- Amazon S3 bucket
- Pinecone account and API key
- PostgreSQL database
- RIUS API key, if observability is enabled

### 1. Clone the Repository

```bash
git clone YOUR_SYNORA_GITHUB_REPOSITORY_URL

cd SYNORA-Enterprise-AI
```

Replace the repository URL with your actual GitHub repository URL.

### 2. Create a Conda Environment

```bash
conda create -n synora python=3.11 -y

conda activate synora
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the project root.

Configure the required environment variables:

```env
# AWS Configuration
AWS_ACCESS_KEY_ID=your_aws_access_key_id
AWS_SECRET_ACCESS_KEY=your_aws_secret_access_key
AWS_REGION=ap-southeast-2
S3_BUCKET=your_s3_bucket_name

# Amazon Bedrock Model Configuration
BEDROCK_EMBED_MODEL=amazon.titan-embed-text-v2:0
BEDROCK_CHAT_MODEL=amazon.nova-lite-v1:0
EMBED_DIM=1024

# Pinecone Vector Database Configuration
PINECONE_API_KEY=your_pinecone_api_key
PINECONE_INDEX_HOST=your_pinecone_index_name
PINECONE_CLOUD=aws
PINECONE_REGION=your_pinecone_supported_region
PINECONE_READY_TIMEOUT=180

# PostgreSQL Database Configuration
DATABASE_URL=your_postgresql_database_url

# RIUS Observability Configuration
RIUS_API_KEY=your_rius_api_key
RIUS_SERVICE_NAME=synora-enterprise-ai
RIUS_DISABLED=false
RIUS_CAPTURE_CONTENT=false
```

Replace all placeholder values with your actual configuration.

**Security:** Never commit your `.env` file, AWS credentials, database passwords, or API keys to GitHub.

### 5. Configure AWS Services

Ensure that:

- Your AWS credentials have the required permissions.
- The configured S3 bucket exists.
- Amazon Bedrock model access is enabled for the selected models.
- The selected AWS region supports the required services.
- The IAM permissions allow the application to access S3 and Bedrock.

### 6. Configure Pinecone

Create or select a Pinecone index with the following configuration:

| Setting | Value |
|---|---|
| Index Name | Configured through `PINECONE_INDEX_HOST` |
| Cloud Provider | AWS |
| Region | Configured through `PINECONE_REGION` |
| Dimension | 1024 |
| Metric | Cosine |

Make sure the Pinecone region matches the region of the actual index.

The Pinecone region is independent of the AWS region used for S3 and Bedrock.

### 7. Configure PostgreSQL

Provide a valid PostgreSQL connection string through `DATABASE_URL`.

Example format:

```env
DATABASE_URL=postgresql://username:password@hostname:5432/database_name
```

For a hosted PostgreSQL database, use the connection details provided by your database provider.

### 8. Run the Application

From the project root, run:

```bash
uvicorn app.main:app --reload
```

Open your browser:

```text
http://127.0.0.1:8000
```

The application should now be accessible locally, provided the required services and configuration are available.

---

## 🔐 Security Considerations

SYNORA is designed around enterprise document workflows, so security is an important part of its architecture.

Important considerations include:

- Keep AWS credentials and third-party API keys in environment variables.
- Apply least-privilege IAM permissions.
- Protect document access using appropriate authorization checks.
- Enforce tenant isolation during vector retrieval.
- Avoid exposing sensitive document content in logs.
- Secure PostgreSQL connections.
- Validate uploaded documents and file types.
- Rotate credentials if they are accidentally exposed.

Production deployment should include complete authentication, authorization, tenant-isolation testing, and secure secret management.

---

## ⚠️ Limitations and Notes

- Answer quality depends on document quality, chunking, embedding generation, and retrieval relevance.
- Amazon Nova Lite generates answers based on the context supplied to it; generated answers should be validated against source documents.
- Pinecone retrieval quality depends on the selected embedding model and retrieval configuration.
- AWS, Pinecone, PostgreSQL, and RIUS availability and API limits can affect execution.
- Correct configuration of the Pinecone index name, cloud provider, region, and embedding dimension is essential.
- Production readiness requires comprehensive testing of authentication, authorization, tenant isolation, and failure handling.

---

## 🚀 Future Enhancements

Potential improvements include:

- Advanced document parsing for tables, images, and scanned PDFs.
- Hybrid search combining semantic and keyword retrieval.
- Reranking for improved retrieval relevance.
- Retrieval evaluation using precision, recall, and faithfulness metrics.
- Streaming responses for improved user experience.
- Advanced role-based access control.
- More comprehensive audit and compliance reporting.
- Automated unit and integration testing.
- Improved monitoring and performance optimization.
- Scalable cloud deployment and infrastructure automation.

---

## 👩‍💻 Author

**Purnima Nallamilli**

B.Tech — Information Technology

Interested in Generative AI, Retrieval-Augmented Generation, Multi-Agent Systems, Cloud Computing, and Backend Development.

- **GitHub:** [Purnima Nallamilli](https://github.com/Purnima-nallamilli)
- **Project:** SYNORA Enterprise AI

---

<p align="center">
  <strong>SYNORA Enterprise AI</strong>
  <br/>
  Turning enterprise documents into actionable knowledge through Generative AI and intelligent retrieval.
</p>
