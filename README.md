# 🔗 API Gateway

Microservicio de entrada para CryptoLens. Único punto de acceso desde el frontend, gestiona autenticación, autorización y enrutamiento.

## Responsabilidad

Expone todos los endpoints REST y WebSocket del sistema. Valida JWT en cada petición y se comunica internamente con el resto de microservicios.

## Endpoints principales

| Grupo | Endpoints |
|---|---|
| Auth | POST /v1/auth/register, login, logout, refresh |
| Usuario | GET/PUT /v1/users/me, preferencias |
| Mercado | GET /v1/market/{symbol}/price, history, indicators, summary |
| Señales | GET /v1/signals/{symbol}, history, accuracy, backtest |
| Chat | POST /v1/chat/message, GET history, daily-summary |
| Noticias | GET /v1/news, /v1/sentiment/{symbol} |
| Alertas | CRUD /v1/alerts |
| Favoritos | GET/POST/DELETE /v1/favorites |
| WebSocket | /v1/ws/price/{symbol}, /v1/ws/alerts/{user_id} |
| Admin | GET /v1/health, /v1/admin/models, jobs |

## Autenticación

- JWT con `python-jose`
- Sesiones en Redis con TTL de 24 horas
- Header requerido: `Authorization: Bearer {token}`

## Stack

- `FastAPI` — framework principal con Swagger automático
- `Pydantic` — validación de datos
- `python-jose` — generación y validación de JWT
- `SQLAlchemy` — ORM para PostgreSQL
- `Redis` — cache de precios y sesiones
- `kafka-python` — consumer de alertas disparadas

## Configuración

```bash
cp .env.example .env
docker compose up api-gateway
```

## Documentación API

Una vez levantado, disponible en:
```
http://localhost:8000/docs
```

## Parte de CryptoLens

[crypto-lens-img](https://github.com/crypto-lens-img) · [data-pipeline](https://github.com/crypto-lens-img/data-pipeline) · [ml-engine](https://github.com/crypto-lens-img/ml-engine) · [llm-service](https://github.com/crypto-lens-img/llm-service)
