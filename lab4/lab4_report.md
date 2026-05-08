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
flowchart TD
    User[👤 Пользователь] -->|HTTPS| Run[☁️ Cloud Run\nFrontend + Backend]
    Run -->|AI-запросы| Gemini[🧠 Gemini API]
    Run -->|Данные| Fire[📊 Firestore]
    Run -->|Файлы| Storage[💾 Cloud Storage]
    
    classDef cloud fill:#4285F4,color:white
    class Run,Fire,Storage,Gemini cloud

flowchart TD
    User[👥 Пользователи / Партнёры] --> LB[🌐 Cloud Load Balancer]
    LB --> FE[☁️ Cloud Run Frontend]
    LB --> BE[☁️ Cloud Run Backend\nv1 + v2]
    
    BE --> AI[🧠 Vertex AI]
    BE --> DB[🗄️ Cloud SQL\nPostgreSQL]
    BE --> Storage[💾 Cloud Storage]
    
    classDef lb fill:#34A853,color:white
    classDef cloud fill:#4285F4,color:white
    class LB lb
    class FE,BE,AI,DB,Storage cloud

flowchart TD
    User[👥 Пользователи] --> LB[🌐 Load Balancer + CDN]
    LB --> FE[☁️ Cloud Run Frontend]
    LB --> BE[☁️ Backend Services]
    
    BE --> AI[🧠 Vertex AI]
    BE --> DB[🗄️ Cloud SQL + Replicas]
    BE --> Cache[⚡ Memorystore Redis]
    BE --> Storage[💾 Cloud Storage + CDN]
    
    classDef lb fill:#34A853,color:white
    classDef cloud fill:#4285F4,color:white
    classDef cache fill:#FBBC05,color:black
    class LB lb
    class FE,BE,AI,DB,Storage cloud
    class Cache cache


---

**Блок 4 — Экономическая модель**

```markdown
## Экономическая модель

| Стадия              | Основные сервисы                              | Примерная стоимость в месяц | Обоснование |
|---------------------|-----------------------------------------------|-----------------------------|-----------|
| Initial (MVP)       | Cloud Run + Firestore + Gemini API            | 1 500 – 3 000 ₽            | Минимальные затраты, быстрое тестирование |
| Testing             | Cloud Run + Cloud SQL + Load Balancer         | 5 000 – 9 000 ₽            | Надёжность и A/B-тестирование |
| Production          | Полный стек + CDN + Redis                     | 30 000 – 60 000 ₽          | Высокая нагрузка и масштабируемость |

*Цены ориентировочные и зависят от фактического использования.*

## Обоснование выбора сервисов

- **Cloud Run** — идеальный serverless-сервис для быстрого старта и автоматического масштабирования.
- **Firestore → Cloud SQL** — постепенный переход от простой базы к мощной реляционной.
- **Vertex AI / Gemini** — современные AI-модели с отличной интеграцией в Google Cloud.
- **Load Balancer + CDN** — обеспечивают высокую доступность и скорость работы.
- **Redis** — кэширование для максимальной производительности на продакшене.

## Выводы

В ходе лабораторной работы была разработана поэтапная инфраструктура AI-приложения. 

**Главный вывод:** инфраструктуру нужно строить **итеративно**. Начинать с минимального MVP, чтобы быстро проверить идею с минимальными затратами, а затем постепенно добавлять компоненты по мере роста нагрузки и требований.

Такой подход позволяет:
- Экономить бюджет на ранних этапах
- Быстро получать обратную связь
- Минимизировать риски
- Легко масштабировать продукт при успехе
