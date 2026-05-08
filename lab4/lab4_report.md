flowchart TD

A[Пользователь] --> B[Frontend<br/>Cloud Storage]
B --> C[Backend API<br/>Cloud Run]
C --> D[OpenAI API]
C --> E[Firestore]
C --> F[Cloud Storage]
C --> G[Cloud Logging]
