# Storyloom Component-Level Breakdown

This document provides a detailed breakdown of the new and updated components for the Storyloom application.

## 1. Library Search Module

### Frontend (Next.js/React)

- **`LibrarySearch.tsx`**: The main component that integrates the search bar and preview modal.
  - **State**:
    - `query (string)`: Stores the user's search input.
    - `suggestions (Array)`: Holds the search results from the backend.
    - `selectedInspiration (Object)`: The book/author chosen by the user.
    - `isModalOpen (boolean)`: Controls the visibility of the preview modal.
  - **Logic**:
    - Implements debouncing for the search input to avoid excessive API calls.
    - Fetches suggestions from the backend as the user types.
    - Renders the `PreviewModal` when a suggestion is clicked.

- **`TypeaheadSearchBar.tsx`**: A reusable search bar component.
  - **Props**:
    - `value`: The current search query.
    - `onChange`: A function to handle input changes.
    - `suggestions`: An array of suggestions to display.
    - `onSelect`: A function to handle the selection of a suggestion.

- **`PreviewModal.tsx`**: A modal to display details about the selected book or author.
  - **Props**:
    - `isOpen`: A boolean to control the modal's visibility.
    - `onClose`: A function to close the modal.
    - `data`: An object containing the author's bio and a short summary of the book.

### Backend (NestJS)

- **`InspirationController.ts`**:
  - **Endpoint**: `GET /api/inspirations/search`
  - **Query Parameters**: `q` (the search query)
  - **Logic**: Handles incoming search requests and delegates to the `InspirationService`.

- **`InspirationService.ts`**:
  - **Logic**:
    - Fetches data from a third-party API (e.g., Google Books API) or a local database.
    - Formats the data and returns it to the controller.
    - Implements caching to reduce latency and API usage.

## 2. Page Range Selector

### Frontend (Next.js/React)

- **`PageRangeSelector.tsx`**: A component for selecting the target story length.
  - **State**:
    - `selectedRange (string)`: Stores the value of the selected page range (e.g., "50-100").
  - **UI**:
    - A group of radio buttons or a segmented control with the following options:
      - "50–100 pages"
      - "100–200 pages"
      - "200–300 pages"
      - "300+ pages"
    - Each option has a tooltip with a hint (e.g., "Short story," "Novella," "Full novel").
  - **Props**:
    - `onChange`: A function to notify the parent component of the selected range.

## 3. Clean Journal UI

### Frontend (Next.js/React)

- **`JournalEditor.tsx`**: The main text editor for journal entries.
  - **Styling (CSS/Styled-components)**:
    - **Background**: A subtle, repeating linear-gradient or SVG background to create the appearance of a lined notebook.
    - **Font**: A serif or handwriting-style font that is easy to read.
    - **Line Spacing**: Increased line spacing to give the text a more spacious and handwritten feel.
    - **"Calm Mode"**: A toggle that switches to a muted color palette and reduces motion.

- **`EmotionVisualizer.tsx` (Optional)**:
  - **UI**: A small graph or spectrum bar that displays the detected emotion and intensity for the current journal entry.
  - **Props**:
    - `emotionData`: An object containing the emotion and intensity score.

## 4. Updated Story Compilation Logic

### Backend (NestJS)

- **`StoryCompilationController.ts`**:
  - **Endpoint**: `POST /api/stories/compile`
  - **Logic**:
    - Receives the user's journal entries, selected inspiration, and page range.
    - Initiates the story compilation process by calling the `StoryCompilationService`.
    - Returns a job ID to the frontend for status polling.

- **`StoryCompilationService.ts`**:
  - **Logic**:
    - Orchestrates the entire story generation process.
    - **Step 1**: Sends the journal entries to the `EmotionalContextAnalyzer` to get an emotional analysis.
    - **Step 2**: Creates a prompt for the AI service that includes the journal entries, emotional analysis, selected inspiration, and page range.
    - **Step 3**: Sends the prompt to the AI service and stores the job ID.
    - **Step 4**: Provides an endpoint for the frontend to poll the status of the job (`'Analyzing'`, `'Outlining'`, `'Writing Chapter 1…'`).
    - **Step 5**: When the story is complete, it saves the final text to the database and any associated assets to S3.

- **`EmotionalContextAnalyzer.ts`**:
  - **Logic**:
    - Integrates with a third-party NLP service.
    - Analyzes the text of the journal entries to detect the dominant emotions and their intensity.
    - Uses a rich emotional taxonomy (e.g., Joy, Sadness, Anger, Hope, etc.).
    - Returns a structured object with the emotional analysis.
