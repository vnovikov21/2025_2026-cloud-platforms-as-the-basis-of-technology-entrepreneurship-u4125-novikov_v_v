# Лабораторная работа 4. Разработка инфраструктуры MVP AI приложения

**University:** [ITMO University](https://itmo.ru/ru/)  
**Faculty:** [FICT](https://fict.itmo.ru)  
**Course:** [Cloud platforms as the basis of technology entrepreneurship](https://itmo-ict-faculty.github.io/cloud-platforms-as-the-basis-of-technology-entrepreneurship/)  
**Year:** 2025/2026  
**Group:** U4125  
**Author:** Vladimir Novikov  
**Lab:** Lab 4  
**Date of create:** 08.05.2026  
**Date of finished:** 08.05.2026

## Цель работы
Разработать схему инфраструктуры MVP AI-приложения на Google Cloud, спроектировать три стадии развития, рассчитать экономическую модель и обосновать выбор сервисов.

## Описание приложения
Приложение — **AI-ассистент** (интеллектуальный чат-бот), который помогает пользователям генерировать тексты, идеи, анализировать данные и работать с изображениями с помощью моделей Google Gemini и Vertex AI.

**Ожидаемая нагрузка:**
- **Initial (MVP)**: до 100 пользователей в сутки  
- **Testing**: до 1000 пользователей в сутки  
- **Production**: 5000+ пользователей в сутки

## Архитектура инфраструктуры

### 1. Стадия 1 — Initial (MVP)

```mermaid
flowchart LR
    User[Пользователь] --> CloudRun[Cloud Run\nFrontend + Backend]
    CloudRun --> Gemini[Gemini API]
    CloudRun --> Firestore[Firestore]
    CloudRun --> Storage[Cloud Storage]

    style CloudRun fill:#4285F4,stroke:#fff,color:#fff
    style Gemini fill:#34A853,stroke:#fff,color:#fff
    style Firestore fill:#FBBC05,stroke:#000,color:#000
    style Storage fill:#4285F4,stroke:#fff,color:#fff

flowchart TD
    User[Пользователи] --> LB[Cloud Load Balancer]
    LB --> Frontend[Cloud Run Frontend]
    LB --> Backend[Cloud Run Backend\nv1 + v2]
    Backend --> AI[Vertex AI]
    Backend --> DB[Cloud SQL PostgreSQL]
    Backend --> Storage[Cloud Storage]

    style LB fill:#34A853,stroke:#fff,color:#fff
    style Frontend fill:#4285F4,stroke:#fff,color:#fff
    style Backend fill:#4285F4,stroke:#fff,color:#fff

flowchart TD
    User[Пользователи] --> LB[Load Balancer + CDN]
    LB --> Frontend[Cloud Run Frontend]
    LB --> Backend[Backend Services]
    Backend --> AI[Vertex AI]
    Backend --> DB[Cloud SQL + Replicas]
    Backend --> Redis[Memorystore Redis]
    Backend --> Storage[Cloud Storage + CDN]

    style LB fill:#34A853,stroke:#fff,color:#fff
    style Frontend fill:#4285F4,stroke:#fff,color:#fff
    style Backend fill:#4285F4,stroke:#fff,color:#fff
    style Redis fill:#FBBC05,stroke:#000,color:#000


---

**Блок 4 — Экономическая модель**

```markdown
## Экономическая модель

| Стадия              | Основные сервисы                              | Примерная стоимость в месяц | Обоснование |
|---------------------|-----------------------------------------------|-----------------------------|-----------|
| Initial (MVP)       | Cloud Run + Firestore + Gemini API            | 1 500 – 3 000 ₽            | Минимальные затраты, быстрое тестирование |
| Testing             | Cloud Run + Cloud SQL + Load Balancer         | 5 000 – 9 000 ₽            | Надёжность и A/B-тестирование |
| Production          | Полный стек + CDN + Redis                     | 30 000 – 60 000 ₽          | Высокая нагрузка и масштабируемость |

*Цены ориентировочные.*

## Обоснование выбора сервисов

- **Cloud Run** — serverless, удобно масштабируется, оплата только за использование.
- **Firestore → Cloud SQL** — постепенный рост базы данных.
- **Vertex AI / Gemini** — мощные AI-модели Google.
- **Load Balancer + CDN** — стабильность и скорость.
- **Memorystore Redis** — кэширование для высокой производительности.

## Выводы

В ходе лабораторной работы была спроектирована поэтапная инфраструктура AI-приложения. 

**Главный вывод:** Лучше развивать инфраструктуру постепенно — от простого MVP к сложному production-решению. Это позволяет экономить бюджет, быстро тестировать идеи и минимизировать риски при масштабировании.
