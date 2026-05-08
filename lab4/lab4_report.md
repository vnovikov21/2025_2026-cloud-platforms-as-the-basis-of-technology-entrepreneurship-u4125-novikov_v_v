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

Пользователь
↓
Cloud Run (Frontend + Backend)
↓          ↓          ↓
Gemini API   Firestore   Cloud Storage

**Используемые сервисы:** Cloud Run, Firestore, Gemini API, Cloud Storage.

---

### 2. Стадия 2 — Testing с партнёрами


Пользователи
↓
Cloud Load Balancer
↓
├── Cloud Run Frontend
└── Cloud Run Backend (v1 и v2)
↓
├── Vertex AI
├── Cloud SQL (PostgreSQL)
└── Cloud Storage
text


**Используемые сервисы:** Cloud Load Balancer, Cloud Run, Cloud SQL, Vertex AI.

---

### 3. Стадия 3 — Production

Пользователи
↓
Cloud Load Balancing + CDN
↓
├── Cloud Run Frontend
└── Backend Services
↓
├── Vertex AI
├── Cloud SQL (с Read Replicas)
├── Memorystore (Redis)
└── Cloud Storage + CDN

**Используемые сервисы:** Cloud Load Balancing, CDN, Cloud Run, Cloud SQL, Redis, Vertex AI.

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
- **Cloud SQL (PostgreSQL)** — надёжная реляционная база данных на следующих этапах.
- **Vertex AI / Gemini** — мощные AI-модели с удобной интеграцией в Google Cloud.
- **Cloud Load Balancer + CDN** — распределение нагрузки и быстрая доставка контента пользователям.

## Выводы

В рамках лабораторной работы была спроектирована инфраструктура AI-приложения на трёх этапах развития. 

Главный вывод: **не стоит сразу строить сложную и дорогую инфраструктуру**. На этапе MVP важно максимально быстро и дёшево проверить гипотезу. По мере роста пользователей инфраструктура должна постепенно усложняться, добавляя надёжность, масштабируемость и производительность.

Такой поэтапный подход позволяет:
- Существенно экономить бюджет на старте
- Быстро вносить изменения
- Минимизировать риски
- Легко масштабироваться при успехе продукта
