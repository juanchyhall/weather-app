# apps/mobile — App Flutter + Kotlin nativo

> Todavía no existe el proyecto Flutter. Crearlo es la historia **CLIMA-2** del backlog.

## Crear el proyecto (una sola vez)

Desde esta carpeta (`apps/mobile`):

```bash
flutter create --org io.github.juanchyhall --project-name clima --platforms android,ios -a kotlin .
```

- `--org io.github.juanchyhall` → el paquete Android queda `io.github.juanchyhall.clima`.
- `-a kotlin` → el código nativo Android se genera en Kotlin.
- `flutter create` no pisa este README ni el `AGENTS.md` existentes.

## Estructura objetivo

```
apps/mobile/
├── lib/
│   ├── main.dart
│   ├── app/                    # MaterialApp, router, tema
│   ├── core/
│   │   ├── config/             # URL del backend por entorno
│   │   ├── network/            # cliente HTTP y WebSocket
│   │   └── errors/
│   └── features/
│       ├── city_search/        # buscar ciudades (CLIMA-6)
│       ├── favorites/          # guardar ciudades (CLIMA-7)
│       ├── weather/            # clima actual + pronóstico (CLIMA-8)
│       └── live_updates/       # suscripción WebSocket (CLIMA-9)
├── test/                       # misma estructura que lib/
└── android/app/src/main/
    ├── kotlin/io/github/juanchyhall/clima/
    │   ├── MainActivity.kt     # registra el platform channel
    │   └── widget/             # widget de pantalla de inicio (CLIMA-11)
    └── res/                    # layouts y metadatos del widget
```

Cada feature se divide en `data/`, `domain/` y `presentation/` (ver `.cursor/rules/flutter-dart.mdc`).

## Dependencias previstas (a confirmar antes de agregar)

| Necesidad | Paquete propuesto | Historia |
|---|---|---|
| Estado | según `docs/adr/0004-estado-flutter.md` | CLIMA-5 |
| HTTP | `dio` o `http` | CLIMA-5 |
| WebSocket | `web_socket_channel` | CLIMA-9 |
| Persistencia local | `shared_preferences` | CLIMA-7 |

Se agregan con `flutter pub add <paquete>` cuando llegue la historia, no antes.

## Platform channel (Flutter ↔ Kotlin)

| Canal | Método | Dirección | Payload | Uso |
|---|---|---|---|---|
| `io.github.juanchyhall.clima/widget` | `updateWidget` | Dart → Kotlin | `{city, temperature_c, weather_code, updated_at}` | Refrescar el widget |

Cualquier método nuevo se agrega a esta tabla en el mismo PR.

## URL del backend

| Dónde corre la app | URL |
|---|---|
| Emulador Android | `http://10.0.2.2:8080` |
| Simulador iOS | `http://localhost:8080` |
| Dispositivo físico | `http://<IP-de-tu-PC>:8080` |

## Comandos

```bash
flutter pub get
flutter analyze
flutter test
flutter run
```
