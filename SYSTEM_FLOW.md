```mermaid
graph TD
    subgraph "UI Layer (Next.js)"
        A[User Onboards/Logs In] --> B{Library of Inspirations};
        B --> C[Typeahead Search Bar];
        C -- Query --> D[Preview Modal];
        D --> E{Select or Skip};
        E --> F[Journal Editor - Clean UI];

        F --> G[Story Generation Setup];
        G --> H["Select Grouped Page Range <br/>(50-100, 100-200, 200-300+)"];
        H --> I[Initiate Story Compilation];
        I -- Request w/ Auth Token, Journal Data, Inspiration, Page Range --> J_BE;

        K[Progress Bar & Status Updates]
        L[View & Export Final Story <br/> (MD, DOCX, PDF, EPUB)]
    end

    subgraph "Backend (NestJS)"
        J_BE(API Gateway);
        J_BE --> M[Middleware <br/>(CORS, Token Verification)];
        M --> N[Story Controller];
        N -- Save Initial Config --> O_DB[(PostgreSQL)];
        N --> P[Emotional Context Analyzer];
        P -- Process Journal Entries --> P;
        P -- Emotional Arc Data --> N;

        N -- "Compile Request <br/>(Inspiration, Page Range, Emotion Data)" --> Q_AI;

        R[Story Compilation Service] -- Polls Status --> Q_AI;
        R -- "Updates Status <br/>('Analyzing', 'Outlining', 'Writing...')" --> O_DB;
        J_BE -- Poll for Status --> R;
        R -- Send Status --> K;

        S[Story Finalization] -- Fetches Compiled Story --> Q_AI;
        S -- Save Final Story --> O_DB;
        S -- Store Exports --> T_S3[(S3 Storage)];
        J_BE -- Request Export --> S;
        S -- Deliver Export --> L;
    end

    subgraph "AI Services"
        Q_AI(AI Service Endpoint);
        Q_AI -- "1. Analyze Sentiment & Emotion" --> Q_AI;
        Q_AI -- "2. Factor in Authorial Inspiration" --> Q_AI;
        Q_AI -- "3. Adhere to Page Range" --> Q_AI;
        Q_AI -- "4. Generate Story Outline & Chapters" --> Q_AI;
    end

    subgraph "Data Layer"
        O_DB[(PostgreSQL)];
        T_S3[(S3 Storage)];
        U_Redis[(Redis Cache)];
    end

    J_BE -- Read/Write Session Data --> U_Redis;
    N -- Cache Frequent Queries --> U_Redis;

```
