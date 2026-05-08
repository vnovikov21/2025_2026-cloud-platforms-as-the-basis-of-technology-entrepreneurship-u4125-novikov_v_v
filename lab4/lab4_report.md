flowchart TD
    A[Users] --> B[Cloud CDN]
    B --> C[Next.js on Cloud Run]
    C --> D[FastAPI on Cloud Run]
    D <--> E[Gemini / Vertex AI]
    D <--> F[Firestore]
    D <--> G[Cloud Storage]
    D <--> H[Vertex Vector Search]
    style C fill:#e3f2fd
    style D fill:#e3f2fd
