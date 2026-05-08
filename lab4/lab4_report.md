**Готово!** Ниже — полностью готовый текст для твоего Markdown-отчёта. Просто скопируй всё ниже и вставь в файл `lab4-report.md`. Всё будет красиво отображаться на GitHub.

---

```markdown
# Лабораторная работа №4  
**"Разработка инфраструктуры MVP AI приложения"**

### Цель работы
Разработать инфраструктуру прототипа AI-приложения (MVP), спроектировать схемы инфраструктуры для трёх этапов жизненного цикла, рассчитать экономическую модель и обосновать выбор технологий и ресурсов.

### Описание приложения
**Название:** TaskAI — AI-ассистент для управления задачами и заметками.

**Ключевой функционал MVP:**
- Веб-интерфейс для создания и управления задачами
- Обработка естественного языка (LLM) — превращение текста в задачи
- Базовый RAG для персонального контекста пользователя
- Авторизация и хранение данных

**Ожидаемая нагрузка:**
- **Initial:** до 100 активных пользователей (~1–2 тыс. запросов/день)
- **Testing (партнёры):** до 500 пользователей (~10–25 тыс. запросов/день)
- **Production:** 5000+ пользователей (~100–300 тыс. запросов/день)

### Выбранная платформа
**Google Cloud Platform (GCP)** — оптимальный выбор благодаря сильным AI-инструментам (Vertex AI, Gemini), удобному serverless и managed Kubernetes.

### Архитектура приложения

#### 1. Общая схема инфраструктуры (High-Level)

```mermaid
flowchart TD
    subgraph Clients ["Клиенты"]
        Users[Пользователи\nBrowser / Mobile]
    end

    subgraph Frontend ["Frontend"]
        CDN[Cloud CDN]
        NextJS[Next.js App\nCloud Run]
    end

    subgraph Backend ["Backend & AI"]
        API[FastAPI Backend\nCloud Run]
        LLM[Vertex AI / Gemini\nLLM Inference]
        Embedding[Embedding Service]
        RAG[RAG Pipeline]
    end

    subgraph Data ["Данные"]
        DB[(Cloud SQL PostgreSQL\nили Firestore)]
        Vector[Vector DB\npgvector / Vertex Vector Search]
        Storage[(Cloud Storage)]
    end

    subgraph Ops ["Мониторинг & Ops"]
        Monitor[Cloud Monitoring + Logging]
        IAM[IAM & Security]
    end

    Users --> CDN
    CDN --> NextJS
    NextJS --> API
    API <--> LLM
    API <--> Embedding
    API <--> RAG
    API <--> DB
    API <--> Vector
    API <--> Storage
    API <--> Monitor
```

#### 2. Архитектура по этапам

**Этап 1: Initial (MVP)** — Максимально serverless

```mermaid
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
```

**Этап 2: Testing с партнёрами**

```mermaid
flowchart TD
    A[Users] --> B[Cloud CDN]
    B --> C[Next.js on Cloud Run]
    C --> D[FastAPI]
    D <--> E[GKE Autopilot]
    E <--> F[Gemini + Self-hosted small models]
    E <--> G[Memorystore Redis]
    E <--> H[Cloud SQL + pgvector]
    style E fill:#f3e5f5
```

**Этап 3: Production**

```mermaid
flowchart TD
    A[Users] --> B[Cloud CDN + Global Load Balancer]
    B --> C[Next.js — Cloud Run / GKE]
    C --> D[Backend Services]
    D <--> E[GKE Standard/Autopilot\nAutoscaling]
    E <--> F[Dedicated Inference Endpoints\nL4 / A100 GPUs]
    E <--> G[Multi-zone Cloud SQL + Read Replicas]
    E <--> H[Vector Search + Redis]
    E <--> I[Cloud Storage + Backup]
    style E fill:#e8f5e9
```

### Экономическая модель (примерные затраты в месяц, USD, 2026)

| Статья расходов              | Initial (MVP) | Testing     | Production     | Примечание |
|-----------------------------|---------------|-------------|----------------|----------|
| Compute (Cloud Run / GKE)   | 8–15         | 40–80      | 200–600       | Основная статья на проде |
| AI Inference (LLM)          | 10–35        | 60–150     | 400–1200      | Самая дорогая часть |
| База данных + Vector        | 5–12         | 25–50      | 80–200        | - |
| Storage + Networking        | 3–8          | 15–30      | 60–150        | Egress трафик |
| Monitoring & Misc           | 2–5          | 10–20      | 40–80         | - |
| **Итого**                   | **30–75**    | **150–330**| **800–2300**  | - |

**Обоснование выбора стратегии:**
- **Initial** — полностью serverless: минимальные затраты, почти нулевой DevOps, быстрый запуск.
- **Testing** — переход на GKE Autopilot + кэширование: баланс цены и стабильности.
- **Production** — managed Kubernetes с dedicated GPU-инференсом: предсказуемость, низкий latency и контроль затрат (Committed Use Discounts).

### Обоснование выбора GCP
- Лучшие встроенные AI/ML инструменты среди облаков.
- Удобный serverless и Autopilot Kubernetes.
- Хорошие стартовые кредиты и pricing для AI.
- Легко мигрировать на self-hosted модели при необходимости.

### Выводы
Оптимальная стратегия — начинать с serverless-решения на этапе MVP и постепенно переходить к managed Kubernetes по мере роста нагрузки. Такой подход минимизирует затраты на старте и обеспечивает масштабируемость и надёжность на продакшене.

Главные риски затрат — LLM inference и egress. Рекомендуется внедрять кэширование, оптимизацию промптов и мониторинг бюджета.

---

**Готово!**  
Просто скопируй весь текст выше в `.md` файл. Mermaid-диаграммы автоматически отрендерятся на GitHub.

Если хочешь изменить название приложения, нагрузку или облако — скажи, быстро поправлю. Удачи с отчётом!
