# Instrucciones para agentes

Este archivo lo leen Cursor y otros agentes (Claude Code, Codex). Las reglas específicas por tecnología están en `.cursor/rules/`. Si una carpeta tiene su propio `AGENTS.md`, ese manda para esa carpeta.

## Proyecto

Monorepo de una app móvil de clima en tiempo real. Es un proyecto **de aprendizaje**: explicá brevemente el porqué de cada decisión cuando escribas código.

| Carpeta | Tecnología | Rol |
|---|---|---|
| `apps/mobile` | Flutter / Dart | App móvil (Android e iOS) |
| `apps/mobile/android` | Kotlin | Widget nativo y platform channel |
| `services/weather-api` | Kotlin / Ktor | API REST + WebSocket, caché, consulta a Open-Meteo |
| `packages/api-contract` | OpenAPI 3.1 | Contrato entre app y backend |

## Reglas de trabajo

1. **El contrato manda.** Cualquier cambio de endpoint empieza en `packages/api-contract/openapi.yaml`; después se actualizan backend y app. Nunca al revés.
2. **Efecto dominó.** Antes de modificar algo compartido (contrato, modelos, `docker-compose.yml`, workflows), listá qué otras partes lo usan y actualizalas en el mismo cambio o dejalo anotado en el PR.
3. **Cambios chicos.** Un ticket de Jira = una rama = un PR. No mezcles app y backend en el mismo PR salvo que el ticket lo pida.
4. **No instales dependencias sin avisar.** Si hace falta un paquete nuevo (pub o Gradle), proponelo y esperá confirmación.
5. **Nada de secretos en el repo.** Variables en `.env` (ignorado); documentarlas en `.env.example`.
6. **Tests junto al cambio.** Toda lógica nueva lleva su test.

## Comandos habituales

```bash
# App
cd apps/mobile && flutter pub get && flutter analyze && flutter test

# Backend
cd services/weather-api && ./gradlew test && ./gradlew run

# Todo en Docker
docker compose up --build
```

## Convenciones

- Ramas: `feature/CLIMA-123-descripcion-corta`, `fix/CLIMA-123-...`
- Commits: `CLIMA-123 feat(mobile): descripción` (ver `docs/03-flujo-trabajo.md`)
- Idioma: código y nombres en inglés; documentación y mensajes al usuario en español.
