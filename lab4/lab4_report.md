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
Разработать схему инфраструктуры MVP AI-приложения на Google Cloud Platform, спроектировать три стадии развития продукта, рассчитать примерную стоимость и обосновать выбор используемых сервисов.

## Описание приложения
Приложение — **AI-ассистент** (чат-бот), который помогает пользователям генерировать тексты, идеи, анализировать данные и работать с изображениями с помощью моделей Gemini / Vertex AI.

Ожидаемая нагрузка:
- **Initial (MVP)**: до 100 активных пользователей в сутки
- **Testing (с партнёрами)**: до 1000 активных пользователей в сутки
- **Production**: от 5000+ активных пользователей в сутки

## Архитектура инфраструктуры

### 1. Стадия 1 — Initial (MVP)
```mermaid
graph TD
    User[Пользователь] --> CloudRun[Cloud Run<br/>Frontend + Backend]
    CloudRun --> Gemini[Gemini API / Vertex AI]
    CloudRun --> Firestore[Firestore Database]
    CloudRun --> Storage[Cloud Storage]
    
    subgraph "Stage 1: Initial MVP"
    CloudRun
    Gemini
    Firestore
    Storage
    end

graph TD
    User[Пользователи] --> LB[Cloud Load Balancer]
    LB --> Frontend[Cloud Run - Frontend]
    LB --> Backend1[Cloud Run - Backend v1]
    LB --> Backend2[Cloud Run - Backend v2]
    
    Backend1 & Backend2 --> AI[Vertex AI]
    Backend1 & Backend2 --> DB[Cloud SQL PostgreSQL]
    Backend1 & Backend2 --> Storage[Cloud Storage]
    
    subgraph "Stage 2: Partner Testing"
    LB
    Frontend
    Backend1
    Backend2
    DB
    end

graph TD
    User[Пользователи] --> LB[Cloud Load Balancing + CDN]
    LB --> Frontend[Cloud Run Frontend]
    LB --> Backend[Backend Services]
    
    Backend --> AI[Vertex AI]
    Backend --> DB[Cloud SQL + Read Replicas]
    Backend --> Cache[Memorystore Redis]
    Backend --> Storage[Cloud Storage + CDN]
    
    subgraph "Stage 3: Production"
    LB
    Frontend
    Backend
    AI
    DB
    Cache
    end


---

**Блок 4 — Экономическая модель**

```markdown
## Экономическая модель

| Стадия              | Основные сервисы                              | Примерная стоимость в месяц | Обоснование |
|---------------------|-----------------------------------------------|-----------------------------|-----------|
| Initial (MVP)       | Cloud Run + Firestore + Gemini API            | 1 500 – 3 000 ₽            | Минимальные затраты, быстрое тестирование идеи |
| Testing             | Cloud Run + Cloud SQL + Load Balancer         | 5 000 – 9 000 ₽            | Надёжность и возможность A/B-тестирования |
| Production          | Full stack + CDN + Redis + Monitoring         | 30 000 – 60 000 ₽          | Масштабируемость и стабильность при большой нагрузке |

*Цены приблизительные и зависят от региона и реальной нагрузки.

## Обоснование выбора сервисов

- **Cloud Run** — serverless, автоматически масштабируется, платить только за реальное использование.
- **Firestore** на старте — быстро и дёшево для небольшого объёма данных.
- **Cloud SQL (PostgreSQL)** на следующих этапах — надёжная реляционная база.
- **Vertex AI / Gemini** — мощные AI-модели с удобной интеграцией.
- **Cloud Load Balancer + CDN** — распределение нагрузки и быстрая доставка контента.

## Выводы

В рамках лабораторной работы была спроектирована инфраструктура AI-приложения на трёх этапах развития. 

Главный вывод: **не стоит сразу строить сложную и дорогую инфраструктуру**. Лучше начинать с минимального и дешёвого решения (MVP), а затем постепенно масштабировать систему по мере роста нагрузки и требований.

Такой подход позволяет экономить бюджет, быстро тестировать гипотезы и минимизировать риски.

Mermaid-диаграммы отлично отображаются на GitHub.
