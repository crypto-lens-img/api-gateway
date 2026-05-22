# API Gateway Service

## Responsabilidad
Unico punto de entrada al sistema desde el frontend.
Autenticacion, autorizacion y enrutamiento de peticiones a los demas servicios.

## Base URL
https://api.cryptolens.duckdns.org/v1/
Versionado /v1/ para poder evolucionar sin romper clientes.

## Autenticacion
- JWT con python-jose
- Sesiones almacenadas en Redis con TTL de 24 horas
- Endpoints publicos: POST /v1/auth/register y POST /v1/auth/login
- Todo lo demas requiere JWT en header: Authorization: Bearer {token}
- Endpoints /v1/admin/* requieren JWT con rol admin

## Endpoints principales
- /v1/auth/*: registro, login, logout, refresh
- /v1/users/me: perfil y preferencias del usuario
- /v1/market/{symbol}/*: precio, historial, indicadores, summary
- /v1/signals/{symbol}/*: senal actual, historial, accuracy, backtest
- /v1/chat/*: mensajes LLM, historial, resumen diario
- /v1/news/*: noticias y sentimiento
- /v1/alerts/*: gestion de alertas
- /v1/favorites/*: cryptos favoritas
- /v1/ws/price/{symbol}: WebSocket precio en tiempo real
- /v1/ws/alerts/{user_id}: WebSocket alertas disparadas
- /v1/health: estado de todos los servicios
- /v1/admin/*: modelos ML y tareas programadas

## Base de datos - Tablas que gestiona
- users
- user_preferences
- user_favorites
- alerts
- alert_history

## Stack
- FastAPI: framework principal, async nativo, Swagger automatico
- Pydantic: validacion de datos en todos los endpoints
- python-jose: generacion y validacion de JWT
- SQLAlchemy: ORM para PostgreSQL
- Redis: cache de precios y sesiones JWT
- kafka-python: consumer de alertas disparadas
- httpx: llamadas internas a otros servicios

## Estructura de carpetas esperada
api-gateway/
  src/
    routers/
      auth.py
      users.py
      market.py
      signals.py
      chat.py
      news.py
      alerts.py
      favorites.py
      admin.py
    websockets/
      price.py
      alerts.py
    middleware/
      auth.py
    db/
      models.py
      session.py
    cache/
      redis.py
  main.py
  requirements.txt
  Dockerfile
  .env.example

## Variables de entorno necesarias
- DATABASE_URL
- REDIS_URL
- JWT_SECRET_KEY
- KAFKA_BOOTSTRAP_SERVERS
- ML_ENGINE_URL
- LLM_SERVICE_URL
- SCHEDULER_URL
