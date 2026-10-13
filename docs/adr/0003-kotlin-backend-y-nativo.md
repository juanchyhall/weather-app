# ADR 0003 — Kotlin en backend y en Android nativo

- **Estado:** aceptado
- **Fecha:** 2026-09-17

## Decisión

Kotlin cumple dos roles:

1. **Backend** (`services/weather-api`) con Ktor: REST + WebSocket, corrutinas, Ktor Client para Open-Meteo.
2. **Nativo Android** (`apps/mobile/android`): widget de pantalla de inicio y un `MethodChannel` para recibir datos de Flutter.

## Motivos

- Cubre los dos usos de Kotlin que suelen pedir las vacantes mobile: integración nativa y servicios.
- El platform channel obliga a entender el límite Flutter ↔ plataforma.

## Consecuencias

- Dos toolchains Kotlin (Gradle del backend y Gradle de Android). Se mantienen separadas; no comparten `build.gradle`.
- JDK 21 para ambos.
- Cursor tiene soporte de Kotlin más limitado que Android Studio / IntelliJ: para depurar Gradle o el widget conviene abrir esa carpeta también en Android Studio.
