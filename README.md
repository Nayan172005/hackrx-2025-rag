# 🚀 HackRx Enhanced RAG API

An **Enhanced Retrieval-Augmented Generation (RAG)** system for processing **insurance policy and legal PDFs**.  
Built with **FastAPI**, **LangChain**, **SentenceTransformers**, **PostgreSQL + pgvector**, **Pinecone**, and **OpenAI gpt-5-mini** for accurate, context-grounded answers.

---

## ✨ Features

- 📄 **PDF Ingestion** – Downloads and extracts text from URLs using `LangChain PyPDFLoader`.
- ✂ **Smart Chunking** – Optimized chunking for legal documents with overlap and custom separators.
- 🔍 **Hybrid Retrieval** – Combines **PostgreSQL vector search** and **Pinecone** for high recall.
- 🧠 **LLM-Optimized Prompting** – Structured prompts for maximum accuracy and conciseness.
- 🛡 **Bearer Token Authentication** – Secure API access via `HTTPBearer`.
- 📦 **Document Caching** – Avoids reprocessing with database-backed cache.
- 📊 **Cache Statistics** – View document and chunk stats via `/cache/stats`.
- 🧾 **Strict JSON Output** – LLM responses strictly follow `{ "answers": [...] }` format.

---

## 🛠 Tech Stack

| Component                  | Technology Used |
|----------------------------|-----------------|
| **Framework**              | FastAPI         |
| **Document Processing**    | LangChain       |
| **Embeddings**             | SentenceTransformers (`all-MiniLM-L6-v2`) |
| **Database**               | PostgreSQL + `pgvector` |
| **Vector Store**           | Pinecone        |
| **LLM**                    | OpenAI gpt-5-mini (configurable) |
| **Auth**                   | HTTP Bearer     |
| **Environment Management** | python-dotenv   |

---

## 📂 Project Structure

```bash
├── main.py
├── README.md
├── requirements.txt
└── report
    └── Pitch Desk.pdf
```

---

## 🛠 Installation & Setup

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/Nayan172005/hackrx-2025-rag.git
cd hackrx-enhanced-rag
```

### 2️⃣ Install Dependencies
```bash
pip install -r requirements.txt
```

### 3️⃣ Set Environment Variables
Create a `.env` file in the root directory:

```env
DATABASE_URL=postgresql+psycopg2://user:password@localhost:5432/yourdb
BEARER_TOKEN=your_secure_token
PINECONE_API_KEY=your_pinecone_api_key
PINECONE_ENVIRONMENT=your_pinecone_env
PINECONE_INDEX=your_index_name
OPENAI_API_KEY=your_openai_key
```
Note: Replace the placeholder values above with your own credentials before running the API.

### 4️⃣ Run the API
```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

---

## 📡 API Endpoints

### 🔍 Process Document & Answer Questions
**POST** `/api/v1/hackrx/run`

**Request Body**
```json
{
  "documents": "https://example.com/policy.pdf",
  "questions": [
    "What is the claim settlement process?",
    "What is the grace period for premium payment?"
  ]
}
```

**Headers**
```makefile
Authorization: Bearer YOUR_TOKEN
```

**Response**
```json
{
  "answers": [
    "The claim settlement process requires submission of all necessary documents within 30 days.",
    "The grace period is 15 days from the due date."
  ]
}
```

---

### 🩺 Health Check
**GET** `/api/v1/health`
```json
{
  "status": "healthy",
  "message": "Enhanced HackRx RAG API is running"
}
```

---

### 📊 Cache Statistics
**GET** `/api/v1/cache/stats`
```json
{
  "cached_documents": 3,
  "total_chunks": 512,
  "cache_status": "active",
  "version": "2.1.0 - Enhanced Retrieval"
}
```

---

## 🔑 Authentication
- The API uses **Bearer Token Authentication**.  
- Pass your token in the `Authorization` header:

```makefile
Authorization: Bearer YOUR_TOKEN
```

---

## 📜 How It Works

1. **Document Download & Extraction**  
   Downloads PDF → Extracts text using **LangChain PyPDFLoader**.

2. **Chunking**  
   Splits text into overlapping chunks optimized for legal/insurance documents.

3. **Embedding Generation**  
   Converts chunks to vector embeddings using **SentenceTransformer (all-MiniLM-L6-v2)**.

4. **Storage**  
   Stores embeddings in **PostgreSQL (pgvector)** and **Pinecone** for retrieval.

5. **Hybrid Retrieval**  
   Queries both PostgreSQL and Pinecone to fetch the most relevant chunks.

6. **LLM Answering**  
   Passes retrieved context + question to **OpenAI gpt-5-mini model** → returns concise, factual answers.

---

## 📌 Notes
- **Model:** `SentenceTransformer(all-MiniLM-L6-v2)` is used for embeddings.  
- **Fallback:** If PostgreSQL returns no matches, Pinecone is used as backup.  
- **Strict JSON:** Output format is always:
```json
{ "answers": [ ... ] }
```

---

