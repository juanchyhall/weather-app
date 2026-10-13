# packages/api-contract — Contrato OpenAPI

`openapi.yaml` es la **fuente de verdad** entre la app y el backend.

Orden obligatorio al cambiar un endpoint:

1. Editar `openapi.yaml` (PR con alcance `contract`).
2. Actualizar el backend (`services/weather-api`).
3. Actualizar la app (`apps/mobile`).

Para verlo: abrir el archivo en Cursor con una extensión de OpenAPI, o pegarlo en <https://editor.swagger.io>.

El WebSocket `/v1/live` no se puede describir completo en OpenAPI; sus mensajes están documentados en la sección `x-websocket-messages`.
