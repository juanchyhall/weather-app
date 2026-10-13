# Backlog inicial (para cargar en Jira)

Proyecto Jira: `CLIMA`. Las claves son sugeridas; Jira asigna las reales al crear.

## Épicas

| Épica | Objetivo |
|---|---|
| E1 · Fundaciones | Repo, proyectos base, Docker y CI funcionando |
| E2 · Clima por ciudad | Buscar, guardar y ver el clima de ciudades |
| E3 · Tiempo real | Actualizaciones por WebSocket |
| E4 · Nativo Android | Widget en Kotlin |

---

## E1 · Fundaciones

### CLIMA-1 · Subir estructura base a GitHub
- [ ] Repo creado con `main` y `develop` protegidas.
- [ ] Cursor muestra las 6 reglas y los 3 subagentes del proyecto.
- [ ] Integración GitHub for Jira activa.

### CLIMA-2 · Crear proyecto Flutter
- [ ] `apps/mobile` generado con `flutter create` (paquete `io.github.juanchyhall.clima`, Kotlin).
- [ ] Estructura `app/`, `core/`, `features/` creada.
- [ ] `flutter analyze` y `flutter test` en verde; `mobile-ci` corre en el PR.

### CLIMA-3 · Crear backend Ktor con `/v1/health`
- [ ] Proyecto Gradle en `services/weather-api` (start.ktor.io).
- [ ] `GET /v1/health` responde según el contrato.
- [ ] Test con `testApplication`; `api-ci` en verde.

### CLIMA-4 · Levantar backend + Redis con Docker
- [ ] `docker compose up --build` levanta ambos servicios.
- [ ] `http://localhost:8080/v1/health` responde `status: ok`.
- [ ] `.env` creado a partir de `.env.example` (no versionado).

### CLIMA-5 · Base de red y estado en Flutter
- [ ] ADR 0004 decidido y regla `flutter-dart.mdc` actualizada.
- [ ] Cliente HTTP con URL por entorno (emulador / dispositivo).
- [ ] Pantalla de prueba que muestra el resultado de `/v1/health`.

## E2 · Clima por ciudad

### CLIMA-6 · Buscar ciudades
- [ ] Backend: `GET /v1/cities/search` usando geocodificación de Open-Meteo, con caché.
- [ ] App: buscador con resultados; estado vacío y de error.
- [ ] Tests en ambos lados.

### CLIMA-7 · Ciudades favoritas
- [ ] Agregar/quitar favoritas desde la búsqueda.
- [ ] Persisten al cerrar la app.
- [ ] Una favorita se marca como "principal" (para el widget).

### CLIMA-8 · Clima actual y pronóstico
- [ ] Backend: `GET /v1/weather` con `current` y `daily`; `source` indica `live` o `cache`.
- [ ] App: pantalla con temperatura, sensación térmica, viento, humedad y 7 días.
- [ ] Ícono y texto en español según `weather_code` (WMO).

## E3 · Tiempo real

### CLIMA-9 · WebSocket `/v1/live`
- [ ] Backend: acepta `subscribe`, responde `update` inmediato y refresca cada `WEATHER_REFRESH_SECONDS`.
- [ ] Solo envía si el dato cambió.
- [ ] App: se suscribe a las favoritas y actualiza la UI sin recargar.

### CLIMA-10 · Reconexión
- [ ] La app reintenta con espera creciente si se corta la conexión.
- [ ] Indicador visual "en vivo / reconectando".

## E4 · Nativo Android

### CLIMA-11 · Widget de pantalla de inicio
- [ ] `MethodChannel` `io.github.juanchyhall.clima/widget` con método `updateWidget`.
- [ ] Widget Kotlin muestra ciudad principal, temperatura y hora de actualización.
- [ ] Se actualiza cuando llega un `update` por WebSocket.

## Futuro (sin priorizar)

- CLIMA-12 · Autenticación y límites de uso en el backend.
- CLIMA-13 · Alertas (lluvia, temperaturas extremas) con notificaciones push.
- CLIMA-14 · Despliegue del backend en la nube.
- CLIMA-15 · Modo sin conexión (último dato guardado).
