# ParsLiveDub Android

**Native Android** product for real-time live dubbing — inspired by the [ParsLiveDub](https://github.com/mo3iiibest77-hub/ParsLiveDub) Chrome extension, built as a **separate commercial app** with a modern stack and richer product features.

> Extension repo stays as-is. This app is not a port of the extension; it reuses the *product idea* (live dubbing via Gemini) with native media control, auth, onboarding, and localization.

## Status

**Phase 0 — Bootstrap** (this commit)

## Official stack (2026)

| Layer | Choice |
|--------|--------|
| Language | **Kotlin** only |
| UI | **Jetpack Compose** + **Material 3** |
| Architecture | MVVM + clean layers (UI / domain / data) |
| Async | Coroutines + Flow / StateFlow |
| DI | Hilt |
| Media | **Media3 (ExoPlayer)** |
| Network | OkHttp + Kotlin Serialization |
| Auth | Credential Manager + Google Sign-In |
| Preferences | DataStore |
| Background | Foreground Service (media/microphone) + notifications |
| i18n | Android resources + per-app language (AppCompat locale) |

No Java for new code. No XML UI layouts (Compose-first).

## Product goals (high level)

- Google account sign-in / sign-up
- Guided onboarding from first install to first successful dub
- Help user obtain / connect **Gemini API** (clear steps; never hardcode secrets)
- Live dubbing pipeline (capture → translate/TTS → playback) with better A/V control than a browser extension
- App language switcher (UI in many languages, not English-only)
- Target language for dubbing (70+), independent of UI language
- Runs reliably with a visible foreground service while dubbing
- Commercial-ready UX (polish, errors, empty states)

## Relation to the extension

| | Chrome extension | This Android app |
|--|------------------|------------------|
| Capture | `tabCapture` | AudioRecord / MediaProjection / Media3 (as designed per feature) |
| Sync picture | Limited on mobile web | Native player timeline control possible |
| Auth / billing path | Manual API key | Google Sign-In + guided API / future Play billing |
| Background | Offscreen document | Foreground service |

## Docs

- [PROJECT_CONTEXT.md](./PROJECT_CONTEXT.md) — handoff context for AI / developers
- [docs/ROADMAP.md](./docs/ROADMAP.md) — phased delivery
- [docs/ARCHITECTURE.md](./docs/ARCHITECTURE.md) — module layout

## Clone

```bash
git clone https://github.com/mo3iiibest77-hub/ParsLiveDub-Android.git
```

Open in **Android Studio** (latest stable). Gradle wrapper and full module graph land in the next commits.

## License

MIT (same spirit as the extension) — see `LICENSE` when added.
