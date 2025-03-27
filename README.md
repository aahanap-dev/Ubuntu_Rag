# Ubuntu Documentation RAG API

## Overview

This project implements a Retrieval-Augmented Generation (RAG) API for querying Ubuntu documentation using FastAPI. The application provides an intelligent search and answer system that can process natural language queries against a comprehensive Ubuntu documentation knowledge base.

## Features

- **RESTful API Endpoint**: Provides a `/query` endpoint for submitting questions
- **Intelligent Query Processing**: Uses RAG technology to retrieve and generate contextually relevant answers
- **Logging and Error Handling**: Comprehensive logging and robust error management
- **Source Document Tracking**: Returns source documents along with generated answers

## Prerequisites

- Python 3.10+
- Docker (optional, for containerized deployment)
- Required Python libraries (see `requirements.txt`)

## Project Structure

```
ubuntu-rag-api/
│
├── app/
│   ├── app.py            # FastAPI application entry point
│   └── RAGService.py      # RAG system implementation
│
├── Dockerfile             # Docker configuration
├── requirements.txt       # Python dependencies
└── README.md              # Project documentation
```

## Installation

### Local Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/ubuntu-rag-api.git
   cd rag
   ```

2. Create a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

### Docker Deployment

1. Build the Docker image:
   ```bash
   docker build -t ubuntu-rag-api .
   ```

2. Run the container:
   ```bash
   docker run -p 8000:8000 ubuntu-rag-api
   ```

## API Endpoints

### Query Endpoint
- **URL**: `/query`
- **Method**: POST
- **Request Body**:
  ```json
  {
    "question": "What are the steps to install Ubuntu?"
  }
  ```
- **Response**:
  ```json
  {
    "question": "What are the steps to install Ubuntu?",
    "answer": "To install Ubuntu, you need to...",
    "sources": [
      "File Name: installation-guide.md",
      "Content: Detailed installation steps..."
    ]
  }
  ```

### Health Check
- **URL**: `/`
- **Method**: GET
- **Response**:
  ```json
  {
    "message": "RAG API is running!"
  }
  ```

## Configuration

### Logging
- Log file: `rag_api.log`
- Log level: INFO
- Log format: `%(asctime)s - %(levelname)s - %(message)s`

## Error Handling

The API provides robust error handling:
- Logs all errors with detailed traceback
- Returns appropriate HTTP status codes
- Prevents sensitive information exposure




