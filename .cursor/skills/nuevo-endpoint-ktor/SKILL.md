---
name: nuevo-endpoint-ktor
description: Agrega un endpoint al backend Ktor partiendo del contrato OpenAPI. Usar cuando un ticket agrega o cambia una ruta de services/weather-api.
paths: "services/weather-api/**, packages/api-contract/**"
---

# Nuevo endpoint Ktor

1. Verificá que el endpoint exista en `packages/api-contract/openapi.yaml`. Si no, agregalo primero y mostrá el cambio.
2. Creá/actualizá el DTO en `model/` respetando nombres y tipos del contrato.
3. Agregá la ruta en `routes/` delegando en un método de `service/`.
4. Implementá la lógica en `service/` usando `client/` y `cache/`.
5. Escribí el test con `testApplication` y un fake del cliente de Open-Meteo.
6. Ejecutá `./gradlew test`.
7. Listá qué debe cambiar en `apps/mobile` para consumir el endpoint (no lo implementes en este paso).
