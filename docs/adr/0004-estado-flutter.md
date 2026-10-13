# ADR 0004 — Gestión de estado en Flutter

- **Estado:** propuesto (confirmar en CLIMA-5)
- **Fecha:** 2026-09-17

## Opciones

| Opción | A favor | En contra |
|---|---|---|
| **Riverpod** | Poco código repetido, fácil de testear, muy usado | Conceptos propios (providers, ref) |
| **BLoC / Cubit** | Muy estructurado, común en empresas grandes | Más archivos por feature |
| Provider | Simple | Queda corto para WebSocket y varias fuentes |

## Propuesta

Riverpod, **salvo que la vacante indique BLoC**: en ese caso usar BLoC, porque el objetivo es llegar preparado a ese stack.

## Al decidir

- Cambiar el estado a "aceptado".
- Actualizar `.cursor/rules/flutter-dart.mdc` con el nombre de la librería y su patrón de carpetas en `presentation/state/`.
