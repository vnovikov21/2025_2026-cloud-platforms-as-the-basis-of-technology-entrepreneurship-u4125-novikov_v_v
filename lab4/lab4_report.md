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
    A[Пользователь] --> B[Cloud Run<br/>Frontend + Backend]
    B --> C[Gemini API / Vertex AI]
    B --> D[Firestore Database]
    B --> E[Cloud Storage<br/>(изображения)]
    
    subgraph "Stage 1: Initial MVP"
    B & C & D & E
    end
graph TD
    A[Пользователи / Партнёры] --> LB[Cloud Load Balancer]
    LB --> FE[Cloud Run - Frontend]
    LB --> BE1[Cloud Run - Backend v1]
    LB --> BE2[Cloud Run - Backend v2]
    
    BE1 & BE2 --> AI[Vertex AI]
    BE1 & BE2 --> DB[Cloud SQL<br/>(PostgreSQL)]
    BE1 & BE2 --> Storage[Cloud Storage]
    
    subgraph "Stage 2: Partner Testing"
    LB & FE & BE1 & BE2 & DB
    end
graph TD
    A[Пользователи] --> LB[Cloud Load Balancing + CDN]
    LB --> FE[Cloud Run Frontend]
    LB --> BE[Backend Services]
    
    BE --> AI[Vertex AI Model Garden]
    BE --> DB[Cloud SQL + Read Replicas]
    BE --> Cache[Memorystore Redis]
    BE --> Storage[Cloud Storage + CDN]
    
    subgraph "Stage 3: Production"
    LB & FE & BE & AI & DB & Cache
    end
