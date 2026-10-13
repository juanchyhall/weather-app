# Guía de Cursor aplicada a este repo

Cursor es un editor basado en VS Code con agentes de IA integrados. Todo lo que está en `.cursor/` y en `AGENTS.md` se versiona con el repo, así que el equipo entero trabaja con las mismas reglas.

> Los atajos y nombres de menús cambian entre versiones. Si algo no coincide, buscalo en la paleta de comandos (`Ctrl+Shift+P`) o en la documentación oficial: <https://cursor.com/docs>.

---

## 1. Las piezas y dónde viven

| Pieza | Dónde | Para qué | Archivo de ejemplo en este repo |
|---|---|---|---|
| **AGENTS.md** | raíz y subcarpetas | Instrucciones generales en markdown simple. El más cercano al archivo gana. | `AGENTS.md`, `apps/mobile/AGENTS.md` |
| **Rules** | `.cursor/rules/*.mdc` | Reglas con metadatos: cuándo se aplican | `.cursor/rules/` |
| **Subagentes** | `.cursor/agents/*.md` | Agentes especializados que el agente principal delega | `planificador`, `revisor`, `tester` |
| **Skills** | `.cursor/skills/<nombre>/SKILL.md` | Procedimientos paso a paso que el agente carga cuando corresponde | `nueva-feature-flutter`, `nuevo-endpoint-ktor` |
| **Commands** | `.cursor/commands/*.md` | Prompts reutilizables que se lanzan con `/` | `/empezar-ticket`, `/preparar-pr`, `/explicame` |
| **MCP** | `.cursor/mcp.json` | Conectar herramientas externas (GitHub, Jira) | `.cursor/mcp.json` |
| **Hooks** | `.cursor/hooks.json` | Scripts que corren antes/después de acciones del agente | (ejercicio 8) |
| **Ignore** | `.cursorignore` | Archivos que Cursor no lee ni indexa | `.cursorignore` |

### Tipos de regla (`.mdc`)

Cada regla empieza con un bloque de metadatos:

```
---
description: Qué hace la regla y cuándo usarla
globs: apps/mobile/lib/**/*.dart
alwaysApply: false
---
```

| Tipo | Cómo se configura | Ejemplo aquí |
|---|---|---|
| Siempre | `alwaysApply: true` | `00-proyecto.mdc` |
| Por archivos | `globs:` con patrones | `flutter-dart.mdc`, `kotlin-ktor.mdc`, `docker.mdc` |
| Inteligente | solo `description`; el agente decide | `git-jira.mdc` |
| Manual | se invoca con `@nombre-regla` en el chat | cualquiera |

Regla práctica: **reglas = cómo escribir código**; **skills = cómo hacer una tarea**; **subagentes = quién la hace**; **commands = atajos para pedirla**.

---

## 2. Modos de trabajo

| Modo | Para qué | Cuándo usarlo en este repo |
|---|---|---|
| **Ask** | Preguntar sin modificar archivos | Entender código, conceptos de Kotlin/Dart |
| **Plan** | Armar un plan revisable antes de tocar nada | Al empezar cada historia |
| **Agent** | Editar archivos y ejecutar comandos | Implementar una vez aprobado el plan |
| **Edición en línea** (`Ctrl+K`) | Cambiar la selección actual | Ajustes chicos |
| **Tab** | Autocompletado | Siempre activo |

Hábitos recomendados:

- Empezá en **Plan**, revisá, y recién después pasá a **Agent**.
- Dale contexto con `@`: `@AGENTS.md`, `@packages/api-contract/openapi.yaml`, `@apps/mobile/lib/features/weather`.
- Usá los **checkpoints** para volver atrás si el agente se desvía.
- Revisá el diff antes de aceptar. El agente propone; vos decidís.
- Un chat por tarea. Si el chat se alarga, abrí uno nuevo con un resumen.

---

## 3. Primeros pasos (una sola vez)

1. Instalar Cursor desde <https://cursor.com> e iniciar sesión.
2. `File → Open Folder` → `D:\Dev Juan\clima-monorepo`.
3. Aceptar las extensiones recomendadas (`.vscode/extensions.json`): Dart, Flutter, Kotlin, Docker, YAML, OpenAPI.
4. Abrir **Cursor Settings** y revisar:
   - **Rules**: deberían aparecer las 6 reglas del proyecto.
   - **MCP**: aparecen `github` y `atlassian` (ver sección 5).
   - **Indexing**: esperar a que termine de indexar el repo.
5. En el chat, probar: `¿Qué reglas del proyecto tenés activas y qué dice AGENTS.md?`

---

## 4. Ejercicios guiados

Cada ejercicio corresponde a una historia del backlog (`docs/04-backlog-inicial.md`).

### Ejercicio 1 — Ask: entender el repo
En modo **Ask**:
> Explicame la arquitectura de este monorepo usando @docs/01-arquitectura.md y @packages/api-contract/openapi.yaml. ¿Qué pasa cuando la app pide el clima?

Objetivo: ver cómo Cursor usa los `@` como contexto.

### Ejercicio 2 — Plan + Agent: crear el proyecto Flutter (CLIMA-2)
1. Modo **Plan**: `Planificá CLIMA-2 siguiendo @apps/mobile/README.md`.
2. Revisá el plan. Pasá a **Agent** y pedí que lo ejecute.
3. Observá que el agente pide permiso antes de correr `flutter create`.

### Ejercicio 3 — Reglas por archivo
Abrí un `.dart` dentro de `apps/mobile/lib` y preguntá: `¿Qué reglas aplican a este archivo?`. Después abrí `docker-compose.yml` y repetí. Deberías ver `flutter-dart` en un caso y `docker` en el otro.

### Ejercicio 4 — Skill: endpoint de health (CLIMA-3)
> Usá la skill nuevo-endpoint-ktor para implementar GET /v1/health.

Mirá cómo la skill obliga a verificar el contrato primero.

### Ejercicio 5 — Subagentes
> Usá el subagente planificador para CLIMA-6.

Después de implementar:
> Usá el subagente tester y luego el revisor.

`planificador` y `revisor` son de solo lectura (`readonly: true`); `tester` corre en segundo plano (`is_background: true`).

### Ejercicio 6 — Commands
Escribí `/` en el chat y elegí `empezar-ticket`, seguido de `CLIMA-7`.

### Ejercicio 7 — MCP con Jira y GitHub
Con los MCP conectados:
> Leé CLIMA-8 en Jira y resumí los criterios de aceptación.
> Creá la rama y, al terminar, abrí el PR en GitHub y comentá el link en el ticket.

### Ejercicio 8 — Hooks (avanzado)
Crear `.cursor/hooks.json` para bloquear `git push --force`:

```json
{
  "version": 1,
  "hooks": {
    "beforeShellExecution": [
      { "command": ".cursor/hooks/bloquear-force-push.ps1", "matcher": "git push.*--force" }
    ]
  }
}
```

El script recibe el evento en JSON por la entrada estándar y, si termina con código `2`, Cursor bloquea la acción. Pedile al agente que lo escriba y probalo. Documentación: <https://cursor.com/docs/hooks>.

### Ejercicio 9 — Crear tu propia regla
Cuando decidas la librería de estado (ADR 0004), pedile al agente:
> Actualizá @.cursor/rules/flutter-dart.mdc con las convenciones de <librería>.

---

## 5. Configurar MCP

`.cursor/mcp.json` ya tiene dos servidores:

| Servidor | Qué necesita |
|---|---|
| `github` | Variable de entorno de Windows `GITHUB_PERSONAL_ACCESS_TOKEN` con un token de GitHub (permisos: repo, pull requests, issues). Reiniciar Cursor después de crearla. |
| `atlassian` | Nada en el archivo: al activarlo, Cursor abre el navegador para iniciar sesión en Atlassian (OAuth). |

El token **no** va en el archivo: `${env:GITHUB_PERSONAL_ACCESS_TOKEN}` lo lee del sistema.

Crear la variable en Windows (PowerShell):

```powershell
[Environment]::SetEnvironmentVariable("GITHUB_PERSONAL_ACCESS_TOKEN", "<tu-token>", "User")
```

---

## 6. Otras funciones para explorar después

- **Bugbot**: revisión automática de PRs en GitHub. Sus reglas van en `.cursor/BUGBOT.md`.
- **Agentes en la nube**: ejecutan tareas en segundo plano sobre el repo remoto.
- **CLI de Cursor**: usar el agente desde la terminal.
- **Reglas de usuario**: preferencias personales para todos tus proyectos (Settings → Rules).

---

## 7. Buenas prácticas con agentes

1. Contexto explícito (`@archivo`) mejor que "mirá el proyecto".
2. Pedir plan antes de código en cualquier cambio de más de un archivo.
3. Tareas chicas: una historia por chat.
4. Siempre cerrar con un comando de verificación.
5. Si el agente repite un error, convertir la corrección en una regla.
6. Revisar que no agregue dependencias ni toque archivos fuera del alcance.
