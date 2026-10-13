# ADR 0002 — Open-Meteo como fuente de datos

- **Estado:** aceptado
- **Fecha:** 2026-09-17

## Decisión

Usar Open-Meteo:

- Pronóstico: `https://api.open-meteo.com/v1/forecast` (parámetros `latitude`, `longitude`, `current`, `daily`, `timezone`).
- Búsqueda de ciudades: `https://geocoding-api.open-meteo.com/v1/search?name=...`

## Motivos

- Gratis para uso no comercial y sin API key: se puede empezar sin registrarse.
- Cobertura mundial y datos de varios servicios meteorológicos nacionales.

## Consecuencias

- Uso comercial requiere plan pago y API key: si el proyecto pasa a producción, agregar `OPEN_METEO_API_KEY` y cambiar la URL base (ya está parametrizada en `.env.example`).
- Respetar los límites del plan gratuito: por eso el backend cachea y la app nunca llama directo.
- Revisar condiciones vigentes en <https://open-meteo.com/en/pricing> antes de publicar.
