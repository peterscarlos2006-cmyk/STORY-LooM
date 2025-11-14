# Storyloom Technical Architecture (MVP)

This document outlines the high-level technical architecture for the Storyloom platform, a creative writing assistant that transforms journal entries into narrative stories using AI.

## 1. System Overview

Storyloom is a full-stack web application built on the following core technologies:

- **Frontend**: Next.js (React)
- **Backend**: NestJS (Node.js)
- **Database**: PostgreSQL
- **Caching**: Redis
- **Storage**: Amazon S3
- **AI Services**: Third-party APIs (e.g., OpenAI, Cohere)

The architecture is designed to be modular, scalable, and maintainable, with a clear separation of concerns between the frontend, backend, and AI services.

## 2. Core Components

### 2.1. Frontend (Next.js)

The frontend is responsible for the user interface and user experience. It is a single-page application (SPA) built with Next.js and React.

- **Key Features**:
  - User authentication and onboarding
  - Journal editor with a clean, lined interface
  - "Library of Inspirations" search and selection
  - Grouped page range selector for story length
  - Story compilation progress view
  - "Calm Mode" for a muted UI
  - Responsive design for desktop and mobile devices

### 2.2. Backend (NestJS)

The backend is the central hub of the application, handling business logic, data persistence, and communication with AI services.

- **Key Responsibilities**:
  - User and authentication management (JWT-based)
  - CRUD operations for journal entries and stories
  - Caching frequently accessed data with Redis
  - Securely storing user-generated content in PostgreSQL and S3
  - Interacting with third-party AI APIs for story generation
  - Emotional Context Analysis of journal entries

### 2.3. Database (PostgreSQL)

PostgreSQL is the primary database for storing structured data.

- **Schema**:
  - `users`: Stores user information (id, email, password hash, etc.)
  - `journals`: Stores journal entries with associated metadata
  - `stories`: Stores generated stories, including inspiration, page range, and status
  - `emotions`: Stores the results of the Emotional Context Analysis

### 2.4. Caching (Redis)

Redis is used for caching to improve performance and reduce database load.

- **Use Cases**:
  - Caching user sessions
  - Caching frequently accessed journal entries and stories
  - Storing temporary data during the story generation process

### 2.5. Storage (Amazon S3)

Amazon S3 is used for storing large binary files.

- **Use Cases**:
  - Storing user-uploaded images and other media
  - Storing exported stories in various formats (PDF, EPUB, Markdown, .docx)

### 2.6. AI Services

Storyloom integrates with third-party AI services to provide its core functionality.

- **Services**:
  - **Natural Language Processing (NLP)**: For sentiment analysis and emotional context extraction
  - **Text Generation**: For creating the story narrative, characters, and dialogue
  - **Image Generation (Optional)**: For creating cover art or illustrations for the story

## 3. New Feature Integration

### 3.1. Universal Library Search

- The frontend will feature a typeahead search bar that queries the backend for authors and books.
- The backend will have an endpoint that searches a dedicated table in the database or a third-party API for book and author information.
- The selected inspiration will be stored with the story metadata.

### 3.2. Grouped Page Range Selector

- The frontend will present the user with a set of predefined page ranges.
- The selected range will be sent to the backend and used to determine the scope of the AI story generation.

### 3.3. Emotional Context Analyzer (ESE)

- The backend will include a new module responsible for analyzing the emotional content of journal entries.
- This module will use an NLP service to detect emotions and assign intensity scores.
- The results will be stored in the `emotions` table and used to influence the tone and style of the generated story.

### 3.4. Updated AI Story Compilation Flow

- The story generation process will be orchestrated by the backend.
- It will take into account the user's selected inspiration, page range, and the emotional context of their journal entries.
- The frontend will poll the backend for status updates and display a progress bar to the user.
