# Entorno local (Windows)

## Requisitos

| Herramienta | Para qué | Verificar |
|---|---|---|
| Cursor | Editor con agentes | abrir la app |
| Git | Control de versiones | `git --version` |
| Flutter SDK (canal stable) | App móvil | `flutter doctor` |
| Android Studio | SDK de Android, emulador, depurar Kotlin | `flutter doctor` lo detecta |
| JDK 21 (Temurin) | Backend Ktor y Gradle de Android | `java -version` |
| Docker Desktop | Backend + Redis | `docker compose version` |
| Node.js (opcional) | MCP locales y lint del contrato | `node -v` |

`flutter doctor` debe quedar sin errores en **Flutter**, **Android toolchain** y **Android Studio** antes de CLIMA-2.

## Primer arranque

```powershell
cd "D:\Dev Juan\clima-monorepo"
copy .env.example .env
```

Luego, según avances en el backlog:

```powershell
# Backend (después de CLIMA-3)
cd services\weather-api
.\gradlew.bat run

# Backend + Redis en Docker (después de CLIMA-4)
docker compose up --build

# App (después de CLIMA-2)
cd apps\mobile
flutter run
```

## Cursor y Kotlin

Cursor edita Kotlin con la extensión recomendada, pero el soporte (autocompletado, Gradle, depurador) es más completo en Android Studio / IntelliJ. Flujo sugerido:

- Cursor para escribir con agentes en todo el repo.
- Android Studio abierto en `apps/mobile/android` o `services/weather-api` para compilar, depurar y ver errores de Gradle.

## Problemas frecuentes

| Síntoma | Causa probable | Solución |
|---|---|---|
| `docker compose up` falla en `gradlew` | El proyecto Ktor no existe todavía | Hacer CLIMA-3 primero |
| `docker compose` dice que falta `.env` | No se copió el ejemplo | `copy .env.example .env` |
| `/bin/sh: ./gradlew: not found` en Docker | Finales de línea CRLF | `.gitattributes` ya lo evita; volver a clonar o `git add --renormalize .` |
| La app no llega al backend en el emulador | Usa `localhost` | Usar `http://10.0.2.2:8080` |
| Android rechaza la conexión HTTP (`Cleartext HTTP traffic not permitted`) | Android bloquea HTTP sin TLS | Permitirlo solo en debug: `android:usesCleartextTraffic="true"` en `android/app/src/debug/AndroidManifest.xml` |
| MCP de GitHub en rojo | Falta la variable de entorno | Crear `GITHUB_PERSONAL_ACCESS_TOKEN` y reiniciar Cursor |
