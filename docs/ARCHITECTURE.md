# Architecture — ParsLiveDub Android

## Module plan (start simple)

```
app/          # UI + DI + app entry
  ui/         # Compose screens
  di/         # Hilt modules
core/
  model/      # pure Kotlin models (optional early)
  common/     # Result, dispatchers
data/
  auth/
  settings/
  gemini/
  media/
domain/
  session/    # start/stop dub session use cases
```

Phase 1 may keep everything under `app/` and split modules when the graph grows.

## Session flow (target)

```
User Start
  → check auth + API key + permissions
  → start ForegroundService
  → AudioCapture → GeminiClient → AudioPlayer
  → UI observes SessionState (Idle|Connecting|Live|Error)
User Stop
  → tear down sockets, release AudioTrack/Record, stop service
```

## Security

- API keys in EncryptedDataStore / Keystore-backed storage only
- Never log full keys
- OAuth client IDs via `local.properties` / CI secrets, not git

## Testing strategy

- Unit: use cases + parsers
- Instrumentation later for permission/session smoke
