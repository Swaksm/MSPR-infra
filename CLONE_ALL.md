# Local development setup

Clone every repository **into the same parent directory** so that docker-compose
relative paths (`../MSPR-backend-*`) resolve correctly.

## 1. Create a workspace directory

```bash
mkdir healthai && cd healthai
```

## 2. Clone all repos

```bash
git clone https://github.com/Sblaaaf/MSPR-infra
git clone https://github.com/Sblaaaf/MSPR-backend-gateway
git clone https://github.com/Sblaaaf/MSPR-backend-auth
git clone https://github.com/Sblaaaf/MSPR-backend-meal
git clone https://github.com/Sblaaaf/MSPR-backend-kcal
git clone https://github.com/Sblaaaf/MSPR-backend-etl
git clone https://github.com/Sblaaaf/MSPR-backend-recommendation
```

Expected layout after cloning:

```
healthai/
├── MSPR-infra/                  ← orchestration (this repo)
│   ├── docker-compose.yml
│   ├── .env.example
│   └── services/
│       └── admin/               ← admin bundled here (no dedicated repo yet)
├── MSPR-backend-gateway/
├── MSPR-backend-auth/
├── MSPR-backend-meal/
├── MSPR-backend-kcal/
├── MSPR-backend-etl/
└── MSPR-backend-recommendation/
```

## 3. Configure environment

```bash
cd MSPR-infra
cp .env.example .env
# Edit .env and fill in RECOMMENDATION_API_KEY and any other secrets
```

## 4. Start the stack

```bash
docker-compose up --build
```

## Service ports

| Service        | Port |
|----------------|------|
| Gateway        | 8000 |
| kcal           | 8001 |
| ETL            | 8002 |
| Meal           | 8003 |
| Auth           | 8004 |
| Admin          | 8005 |
| Recommendation | 8006 |
| Adminer (DB)   | 8080 |
| PostgreSQL     | 5432 |
| MongoDB        | 27017 |

## Notes

- `admin` service lives in `MSPR-infra/services/admin/` — it has not been
  extracted to its own repository yet.
- Trained ML models (recommendation service) are stored in a Docker named
  volume (`recommendation_models`) and auto-generated on first startup.
  Training data is read from `MSPR-backend-etl/data/`.
