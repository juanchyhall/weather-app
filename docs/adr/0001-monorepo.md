# ADR 0001 — Monorepo sin orquestador

- **Estado:** aceptado
- **Fecha:** 2026-09-17

## Contexto

Hay tres piezas (app Flutter, backend Ktor, contrato) con ecosistemas distintos (pub y Gradle). Herramientas como Melos (Dart) o Nx/Turborepo resuelven monorepos de un solo ecosistema.

## Decisión

Un repositorio con carpetas `apps/`, `services/` y `packages/`. Cada pieza conserva su herramienta nativa. La coordinación se hace con:

- CI por carpeta (`paths:` en GitHub Actions).
- `docker-compose.yml` en la raíz.
- `AGENTS.md` y `.cursor/rules/` con reglas por carpeta.

## Consecuencias

- (+) Un PR puede cambiar contrato, backend y app juntos.
- (+) Cursor indexa todo y entiende las dos puntas.
- (−) No hay comando único para "build de todo"; se documenta en cada README.
- Si más adelante hay varios paquetes Dart, evaluar **Melos** solo para `apps/` y `packages/` de Dart.
