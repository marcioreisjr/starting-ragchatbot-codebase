# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Retrieval-Augmented Generation (RAG) system for course materials built with FastAPI backend, ChromaDB vector storage, and Anthropic's Claude AI. The system processes course documents, creates semantic embeddings, and enables intelligent Q&A about course content.

## Key Development Commands

**Setup and Installation:**
```bash
# Install dependencies
uv sync

# Create .env file with ANTHROPIC_API_KEY (see .env.example)
```

**Running the Application:**
```bash
# Quick start (recommended)
chmod +x run.sh
./run.sh

# Manual start (backend only)
cd backend && uv run uvicorn app:app --reload --port 8000

# Run any Python files with uv
uv run python filename.py
```

**Application URLs:**
- Web Interface: http://localhost:8000
- API Documentation: http://localhost:8000/docs

## Architecture

The system uses a modular architecture with clear separation of concerns:

### Core Components (backend/)

- **app.py**: FastAPI application with CORS middleware and API endpoints
- **rag_system.py**: Main orchestrator coordinating all components
- **vector_store.py**: ChromaDB integration with SentenceTransformer embeddings
- **ai_generator.py**: Anthropic Claude API integration for response generation
- **document_processor.py**: Text chunking and course document parsing
- **session_manager.py**: Conversation history management
- **search_tools.py**: Tool-based search system for AI function calling
- **models.py**: Pydantic data models (Course, Lesson, CourseChunk)
- **config.py**: Configuration management with environment variables

### Data Flow

1. Course documents (PDF, DOCX, TXT) are processed into chunks
2. Chunks are embedded using SentenceTransformer and stored in ChromaDB
3. User queries trigger semantic search via AI tools
4. Retrieved context is used by Claude to generate responses
5. Session manager maintains conversation history

### Frontend

Simple HTML/CSS/JavaScript interface in `frontend/` directory served as static files.

## Configuration

Key settings in `config.py`:
- **CHUNK_SIZE**: 800 characters (affects retrieval granularity)
- **CHUNK_OVERLAP**: 100 characters (maintains context between chunks)
- **MAX_RESULTS**: 5 search results per query
- **EMBEDDING_MODEL**: "all-MiniLM-L6-v2" (SentenceTransformer model)
- **ANTHROPIC_MODEL**: "claude-sonnet-4-20250514"

## Document Format

Course documents in `docs/` follow this structure:
```
Course Title: [Title]
Course Link: [URL]
Course Instructor: [Name]

Lesson 0: [Title]
Lesson Link: [URL]
[Lesson content...]
```

## Development Notes

- The system auto-loads documents from `../docs` on startup
- ChromaDB persistence is stored in `./chroma_db` directory
- CORS is enabled for all origins (development configuration)
- Course deduplication prevents re-processing existing courses
- Tool-based search allows AI to query the vector store dynamically