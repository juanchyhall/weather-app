# Weather App — clima de capitales

App móvil que muestra el clima actual y el pronóstico de 7 días de las capitales de los 195 países, con filtros por clima (soleado, lluvioso, frío, cálido, con tormentas…) y capitales favoritas.

> Estado: el backend está en construcción y la app todavía no existe.

## Estructura

| Carpeta | Tecnología | Rol |
|---|---|---|
| `apps/mobile` | Flutter / Dart, con código nativo Android en Kotlin | App móvil (pendiente) |
| `services/weather-api` | Kotlin + Ktor | API REST: capitales, clima y caché sobre Open-Meteo (en construcción) |
| `packages/api-contract` | OpenAPI 3.1 | Contrato entre la app y el backend |
| `docs` | Markdown | Arquitectura y decisiones (ADR) |

El contrato es la fuente de verdad: cualquier cambio de endpoint empieza en `packages/api-contract/openapi.yaml`.

## Requisitos

| Herramienta | Para qué |
|---|---|
| Git | Clonar el repo |
| JDK 21 (Temurin) | Compilar y correr el backend |
| Docker con Compose | Levantar backend y Redis |
| Node.js (opcional) | Validar el contrato |
| Flutter SDK (canal stable) y Android Studio | Solo para la app |

## Primer arranque

```bash
git clone https://github.com/juanchyhall/weather-app.git
cd weather-app
cp .env.example .env    # en Windows: copy .env.example .env
```

`.env` no se versiona. Los valores de `.env.example` sirven para desarrollo local.

## Levantar el backend

Esta sección se completa cuando el proyecto del backend esté creado.

## Validar el contrato

```bash
npx @redocly/cli lint packages/api-contract/openapi.yaml
```

## Problemas frecuentes

| Síntoma | Causa | Solución |
|---|---|---|
| `docker compose` avisa que falta `.env` | No se copió el ejemplo | Copiar `.env.example` a `.env` |
| `/bin/sh: ./gradlew: not found` dentro de Docker | Finales de línea CRLF en un clon de Windows | `.gitattributes` lo evita; en un clon viejo, `git add --renormalize .` |

## Fuente de datos

[Open-Meteo](https://open-meteo.com/): gratis para uso no comercial y sin API key. Los datos se publican bajo CC BY 4.0. Ver `docs/adr/0002-open-meteo.md`.
