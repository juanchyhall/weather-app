# services/weather-api — Backend Kotlin + Ktor

> Todavía no existe el proyecto Gradle. Crearlo es la historia **CLIMA-3** del backlog.

## Qué hace

1. Recibe pedidos de la app (REST) según `packages/api-contract/openapi.yaml`.
2. Consulta Open-Meteo (pronóstico y geocodificación).
3. Guarda las respuestas en caché (Redis) para no repetir llamadas.
4. Mantiene conexiones WebSocket y **empuja** el clima actualizado cada `WEATHER_REFRESH_SECONDS`.

## Crear el proyecto (una sola vez)

Opción recomendada: generador oficial en <https://start.ktor.io>

- Project artifact: `io.github.juanchyhall.clima`
- Build system: Gradle Kotlin
- Engine: Netty · Configuración: YAML
- Plugins: Routing, Content Negotiation + kotlinx.serialization, WebSockets, Call Logging, Status Pages, Ktor Client (CIO)

Descomprimir el contenido **dentro de esta carpeta** sin pisar `README.md`, `Dockerfile` ni `.dockerignore`.

## Estructura objetivo

```
services/weather-api/
├── build.gradle.kts
├── settings.gradle.kts
├── gradlew / gradlew.bat / gradle/
├── Dockerfile
└── src/
    ├── main/
    │   ├── kotlin/io/github/juanchyhall/clima/
    │   │   ├── Application.kt
    │   │   ├── plugins/
    │   │   ├── routes/          # HealthRoutes, CityRoutes, WeatherRoutes, LiveRoutes
    │   │   ├── service/
    │   │   ├── client/          # OpenMeteoClient
    │   │   ├── cache/           # WeatherCache (Redis / memoria)
    │   │   └── model/
    │   └── resources/application.yaml
    └── test/kotlin/io/github/juanchyhall/clima/
```

## Endpoints (resumen del contrato)

| Método | Ruta | Descripción | Historia |
|---|---|---|---|
| GET | `/v1/health` | Estado del servicio | CLIMA-3 |
| GET | `/v1/cities/search?q=` | Buscar ciudades | CLIMA-6 |
| GET | `/v1/weather?lat=&lon=` | Clima actual + pronóstico | CLIMA-8 |
| WS | `/v1/live` | Actualizaciones en tiempo real | CLIMA-9 |

## Comandos

```bash
./gradlew run          # levanta en http://localhost:8080
./gradlew test
./gradlew buildFatJar  # jar que usa el Dockerfile
```

En Windows: `gradlew.bat` en lugar de `./gradlew`.
