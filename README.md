Setu-AI (सेतु-AI) 🚧 Under Active Development
Bridging the gap between theory and industry for Indian Computer Science students.

📖 Overview
Setu-AI is a multilingual RAG (Retrieval-Augmented Generation) system designed to bridge the language gap in computer science education for Indian students. The name "Setu" means "bridge" in Sanskrit, reflecting the system's mission to connect English academic resources with students' native language understanding.

The system allows students to upload standard English PDF textbooks and ask questions in Hindi. It utilizes AWS Cloud services to generate explanations that are not only translated but enriched with real-world analogies relevant to the Indian context.

✨ Key Features
Multilingual RAG Engine: Processes English PDFs but accepts queries in Hindi, generating responses using AWS Bedrock (Claude 3.5 Sonnet).

Vernacular & Analogy Support: Explains complex technical concepts using simple Hindi and culturally relevant analogies.

Smart Citations: Every answer includes specific page references and excerpts from the original PDF, allowing students to verify facts.

Context-Aware Retrieval: Uses Amazon Titan Multilingual Embeddings to match Hindi queries with English textbook content.

Dual Testing Strategy: Reliability is ensured through both Unit Testing and Property-Based Testing (Hypothesis).

🛠️ Architecture
The system follows a microservices pattern with a clear separation between processing, retrieval, and generation.

Code snippet
graph LR
    User[Student (Hindi Query)] --> UI[Streamlit UI]
    UI --> API[FastAPI Backend]
    API --> Bedrock[AWS Bedrock (Claude 3.5)]
    API --> OpenSearch[OpenSearch Serverless]
    
    subgraph "Ingestion Pipeline"
    PDF[PDF Upload] --> Processor[PDF Processor]
    Processor --> Titan[Titan Embeddings]
    Titan --> OpenSearch
    end
Tech Stack
Frontend: Streamlit

Backend: FastAPI

LLM: AWS Bedrock (Claude 3.5 Sonnet)

Embeddings: Amazon Titan Multilingual

Vector Database: Amazon OpenSearch Serverless

Testing: Pytest & Hypothesis

🗺️ Project Roadmap
We are currently rebuilding the application to meet the scalable architecture defined in our Design Document:

[ ] Phase 1: Ingestion Pipeline

Implement PDF text extraction with page tracking.

Set up Amazon Titan embedding generation.

[ ] Phase 2: Core RAG

Configure OpenSearch Serverless.

Build Query Handler for semantic search.

[ ] Phase 3: Generation & UI

Integrate AWS Bedrock (Claude 3.5) for Hindi generation.

Build Streamlit Interface.

[ ] Phase 4: Reliability

Implement Citation Manager.

Add Property-Based Tests.
