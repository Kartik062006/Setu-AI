# Requirements Document

## Introduction

Setu-AI is a RAG-based educational tool designed specifically for Indian Computer Science students. The system enables students to upload English PDF textbooks and ask questions in Hindi, receiving explanations in Hindi with simple real-world analogies. The name "Setu" means "bridge" in Sanskrit, reflecting the system's role in bridging language barriers in technical education.

## Glossary

- **Setu_System**: The complete RAG-based educational application
- **PDF_Processor**: Component responsible for ingesting and processing PDF documents
- **Query_Handler**: Component that processes student questions in Indian languages
- **Retrieval_Engine**: Component that finds relevant content from processed PDFs
- **Response_Generator**: Component that generates explanations using LLM
- **Citation_Manager**: Component that tracks and provides source attribution
- **Student**: End user who uploads PDFs and asks questions
- **Administrator**: User who manages system configuration and monitoring
- **Textbook**: PDF document containing educational content in English
- **Query**: Student question posed in Hindi or other Indian languages
- **Explanation**: Generated response in the requested Indian language with analogies
- **Source_Citation**: Reference to specific page and section of original PDF

## Requirements

### Requirement 1: PDF Document Ingestion

**User Story:** As a student, I want to upload PDF textbooks to the system, so that I can ask questions about the content later.

#### Acceptance Criteria

1. WHEN a student uploads a PDF file, THE PDF_Processor SHALL validate the file format and size constraints
2. WHEN a valid PDF is uploaded, THE PDF_Processor SHALL extract text content while preserving page references
3. WHEN text extraction is complete, THE PDF_Processor SHALL generate embeddings using Amazon Titan Multilingual
4. WHEN embeddings are generated, THE PDF_Processor SHALL store them in Amazon OpenSearch Serverless with page metadata
5. IF a PDF upload fails validation, THEN THE Setu_System SHALL provide clear error messages to the student
6. WHEN PDF processing is complete, THE Setu_System SHALL confirm successful ingestion to the student

### Requirement 2: Multilingual Query Processing

**User Story:** As a student, I want to ask questions in Hindi about English textbook content, so that I can learn concepts in my preferred language.

#### Acceptance Criteria

1. WHEN a student submits a query in Hindi, THE Query_Handler SHALL accept and process the input
2. WHEN processing a Hindi query, THE Query_Handler SHALL generate embeddings using Amazon Titan Multilingual
3. WHEN query embeddings are ready, THE Retrieval_Engine SHALL search for semantically similar content in the vector database
4. WHEN relevant content is found, THE Retrieval_Engine SHALL rank results by relevance score
5. IF no relevant content is found, THEN THE Setu_System SHALL inform the student that the topic is not covered in uploaded materials
6. WHEN multiple relevant passages exist, THE Retrieval_Engine SHALL return the top 3 most relevant sections

### Requirement 3: Vernacular Response Generation

**User Story:** As a student, I want to receive explanations in Hindi with simple analogies, so that I can understand complex computer science concepts easily.

#### Acceptance Criteria

1. WHEN relevant content is retrieved, THE Response_Generator SHALL use AWS Bedrock Claude 3.5 Sonnet to generate explanations
2. WHEN generating responses, THE Response_Generator SHALL translate technical concepts into Hindi
3. WHEN explaining concepts, THE Response_Generator SHALL include real-world analogies relevant to Indian context
4. WHEN creating explanations, THE Response_Generator SHALL maintain technical accuracy while simplifying language
5. WHEN responses are generated, THE Response_Generator SHALL ensure cultural appropriateness for Indian students
6. THE Response_Generator SHALL format responses in clear, structured Hindi text

### Requirement 4: Source Citation and Attribution

**User Story:** As a student, I want to see which page of my textbook contains the information, so that I can refer back to the original source for deeper study.

#### Acceptance Criteria

1. WHEN generating a response, THE Citation_Manager SHALL identify the source page numbers for retrieved content
2. WHEN displaying explanations, THE Setu_System SHALL show page references alongside the generated text
3. WHEN multiple sources contribute to an answer, THE Citation_Manager SHALL list all relevant page numbers
4. WHEN citing sources, THE Setu_System SHALL display the original English text excerpt that was used
5. THE Citation_Manager SHALL maintain accurate mapping between generated content and source locations
6. WHEN students click on citations, THE Setu_System SHALL highlight the relevant section in the original PDF

### Requirement 5: User Interface and Experience

**User Story:** As a student, I want an intuitive web interface, so that I can easily upload documents and ask questions without technical complexity.

#### Acceptance Criteria

1. WHEN a student accesses the system, THE Setu_System SHALL display a clean Streamlit-based interface
2. WHEN uploading files, THE Setu_System SHALL provide drag-and-drop functionality with progress indicators
3. WHEN asking questions, THE Setu_System SHALL provide a text input that supports Hindi typing
4. WHEN processing queries, THE Setu_System SHALL show loading indicators and estimated completion time
5. WHEN displaying responses, THE Setu_System SHALL format text for readability with proper Hindi font rendering
6. THE Setu_System SHALL maintain conversation history for the current session

### Requirement 6: System Performance and Scalability

**User Story:** As an administrator, I want the system to handle multiple concurrent users efficiently, so that it can serve educational institutions effectively.

#### Acceptance Criteria

1. WHEN multiple students upload PDFs simultaneously, THE Setu_System SHALL process them without performance degradation
2. WHEN handling concurrent queries, THE Setu_System SHALL start streaming the response (Time to First Token) within 3 seconds.
3. THE Setu_System SHALL complete the full text generation within 20 seconds for standard queries.
4. WHEN the vector database grows, THE Retrieval_Engine SHALL maintain sub-second search performance
5. WHEN system load increases, THE Setu_System SHALL scale horizontally using AWS serverless capabilities
6. THE Setu_System SHALL support at least 100 concurrent active users
7. WHEN processing large PDFs (up to 50MB), THE PDF_Processor SHALL complete ingestion within 5 minutes

### Requirement 7: Data Security and Privacy

**User Story:** As a student, I want my uploaded documents and queries to be secure, so that my academic materials remain private.

#### Acceptance Criteria

1. WHEN students upload PDFs, THE Setu_System SHALL encrypt files during transmission and storage
2. WHEN storing user data, THE Setu_System SHALL implement access controls to prevent unauthorized access
3. WHEN processing queries, THE Setu_System SHALL not log sensitive academic content
4. WHEN users delete documents, THE Setu_System SHALL remove all associated data including embeddings
5. THE Setu_System SHALL comply with Indian data protection regulations
6. WHEN authentication is required, THE Setu_System SHALL use secure session management

### Requirement 8: Error Handling and Reliability

**User Story:** As a student, I want the system to handle errors gracefully, so that I can continue learning even when technical issues occur.

#### Acceptance Criteria

1. WHEN PDF processing fails, THE Setu_System SHALL provide specific error messages and suggested solutions
2. WHEN the LLM service is unavailable, THE Setu_System SHALL queue requests and notify users of delays
3. WHEN vector search fails, THE Setu_System SHALL attempt fallback search methods
4. WHEN network connectivity issues occur, THE Setu_System SHALL cache responses for offline access
5. IF system components fail, THEN THE Setu_System SHALL log errors for administrator review
6. WHEN recovering from failures, THE Setu_System SHALL preserve user session state and uploaded documents

### Requirement 9: Content Quality and Accuracy

**User Story:** As a student, I want accurate and relevant explanations, so that I can trust the system for my computer science education.

#### Acceptance Criteria

1. WHEN generating explanations, THE Response_Generator SHALL maintain factual accuracy of technical concepts
2. WHEN translating content, THE Response_Generator SHALL preserve the meaning of technical terms
3. WHEN providing analogies, THE Response_Generator SHALL ensure they accurately represent the underlying concepts
4. WHEN content is ambiguous, THE Response_Generator SHALL ask clarifying questions rather than guess
5. THE Response_Generator SHALL be evaluated using "Faithfulness" metrics (checking if the answer is supported by the PDF).
6. IF the AI detects the retrieved context is irrelevant, THE Setu_System SHALL state "I cannot find this in your textbook" instead of hallucinating.

### Requirement 10: System Monitoring and Analytics

**User Story:** As an administrator, I want to monitor system usage and performance, so that I can optimize the service for educational institutions.

#### Acceptance Criteria

1. WHEN students use the system, THE Setu_System SHALL log usage metrics without storing personal information
2. WHEN performance issues occur, THE Setu_System SHALL generate alerts for administrator attention
3. WHEN analyzing usage patterns, THE Setu_System SHALL provide insights on popular topics and query types
4. THE Setu_System SHALL track response accuracy through optional student feedback
5. WHEN system resources are constrained, THE Setu_System SHALL prioritize active learning sessions
6. THE Setu_System SHALL generate daily reports on system health and usage statistics