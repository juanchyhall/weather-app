---
name: nueva-feature-flutter
description: Crea la estructura de una feature nueva en la app Flutter (data/domain/presentation) con sus tests. Usar cuando un ticket agrega una pantalla o flujo nuevo en apps/mobile.
paths: "apps/mobile/**"
---

# Nueva feature Flutter

1. Confirmá el nombre de la feature en `snake_case` (ej. `city_search`).
2. Creá en `apps/mobile/lib/features/<feature>/`:
   - `domain/entities/`, `domain/repositories/`, `domain/usecases/`
   - `data/models/`, `data/datasources/`, `data/repositories/`
   - `presentation/screens/`, `presentation/widgets/`, `presentation/state/`
3. Creá los tests espejo en `apps/mobile/test/features/<feature>/`.
4. Los modelos de `data/models/` salen del esquema correspondiente en `packages/api-contract/openapi.yaml`.
5. Registrá la ruta nueva en `lib/app/` si la feature tiene pantalla.
6. Ejecutá `flutter analyze` y `flutter test` y reportá el resultado.
