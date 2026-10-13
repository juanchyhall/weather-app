# Clima Monorepo

App móvil de clima en tiempo real para aprender a trabajar con **Cursor** (agentes, reglas, skills, MCP) sobre un stack **Flutter + Dart + Kotlin**, con **Docker**, **GitHub** y **Jira**.

> Estado: **estructura base**. No hay código de funcionalidades todavía: se escribe con Cursor siguiendo el backlog de `docs/04-backlog-inicial.md`.

## Qué hace la app (objetivo)

- Buscar ciudades de cualquier región y guardarlas como favoritas.
- Ver el clima actual y el pronóstico de cada ciudad.
- Recibir actualizaciones **en tiempo real** (WebSocket) sin recargar.
- Ver el clima de la ciudad principal en un **widget nativo de Android** (Kotlin).

## Mapa del monorepo

```
clima-monorepo/
├── AGENTS.md                 ← instrucciones generales para cualquier agente de IA
├── .cursor/                  ← configuración de Cursor (versionada con el repo)
│   ├── rules/                ← reglas .mdc (siempre, por archivo o a pedido)
│   ├── agents/               ← subagentes propios (planificador, revisor, tester)
│   ├── skills/               ← procedimientos reutilizables (SKILL.md)
│   ├── commands/             ← comandos /slash propios
│   └── mcp.json              ← conexión a GitHub y Jira vía MCP
├── .cursorignore             ← lo que Cursor no debe indexar
├── apps/
│   └── mobile/               ← app Flutter (Dart) + código nativo Android (Kotlin)
├── services/
│   └── weather-api/          ← backend Kotlin + Ktor (API REST + WebSocket)
├── packages/
│   └── api-contract/         ← contrato OpenAPI compartido entre app y backend
├── docs/                     ← arquitectura, guía de Cursor, flujo de trabajo, backlog, ADRs
├── .github/                  ← CI por carpeta y plantilla de PR
├── .vscode/extensions.json   ← extensiones recomendadas (Cursor las ofrece al abrir)
├── docker-compose.yml        ← backend + Redis para desarrollo local
├── .env.example              ← variables (copiar a .env)
├── .gitattributes            ← finales de línea (evita errores de gradlew en Docker)
├── .editorconfig
└── .gitignore
```

## Por dónde empezar

1. `docs/05-entorno-local.md` — instalar lo necesario.
2. `docs/01-arquitectura.md` — entender cómo encajan las piezas.
3. `docs/02-guia-cursor.md` — aprender Cursor con ejercicios sobre este repo.
4. `docs/03-flujo-trabajo.md` — ramas, commits, PR y Jira.
5. `docs/04-backlog-inicial.md` — cargar las historias en Jira y arrancar por `CLIMA-1`.

## Fuente de datos

[Open-Meteo](https://open-meteo.com/): gratis para uso no comercial y sin API key. Ver `docs/adr/0002-open-meteo.md`.
