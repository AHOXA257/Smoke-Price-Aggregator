# Smoke Price Aggregator (MVP)
Агрегатор цін сировини для копчення з кнопкою "Оновити".

## Швидкий старт (Docker)
```bash
cp .env.sample .env
docker compose up --build
```
- БД: Postgres на `localhost:5432`
- API: FastAPI на `http://localhost:8000`
- UI: Streamlit на `http://localhost:8501`

## Потік даних
1. UI натискає `POST /api/refresh`
2. Backend запускає `scraper/pipeline.py` (stub) → читає `scraper/demo_sources.json` і `scraper/demo_listings.json`, нормалізує та заливає в Postgres
3. Перегляд через `GET /api/listings` або в UI (фільтри + експорт)

## Структура
- `db/` — схема БД і сид-дані
- `backend/` — FastAPI, моделі, CRUD, ендпоінти
- `scraper/` — пайплайн збору/нормалізації (поки stub із демо-даними)
- `ui/` — Streamlit застосунок ("1 кнопка")

## Розробка без Docker
Потрібні Python 3.11+, Postgres.
1) Створіть БД `prices` та виконайте `db/schema.sql` і `db/seed.sql`
2) `cd backend && pip install -r requirements.txt && uvicorn backend.main:app --reload`
3) `cd ui && pip install -r requirements.txt && streamlit run ui/app.py`

## Наступні кроки
- Додати реальні джерела (API/скрейп) у `scraper/sources/*.py` і зареєструвати в `scraper/pipeline.py`
- Увімкнути Redis-кеш у fetchers
- Винести розклад (Prefect/Airflow) — за потреби
