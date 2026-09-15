# Engineering quality bar — ParsLiveDub Android

Target: **commercial, production-grade** Android — not a prototype aesthetic.

## Non-negotiables

### Language & platform
- Kotlin only (no new Java)
- Jetpack Compose + Material 3 (Compose-first)
- Coroutines / Flow / StateFlow for all async
- Hilt for DI
- Strict null-safety; no `!!` without justification comment

### Architecture
- Clear layers: UI → ViewModel → Domain (use cases) → Data
- Unidirectional data flow; UI is a function of state
- Single source of truth for session state
- Feature-oriented packages as the app grows

### Code quality
- Detekt + ktlint (or Android Studio defaults enforced in CI later)
- No god classes; prefer small testable units
- Explicit error types for user-facing failures (network, quota, permission, auth)
- Structured logging (Timber or equivalent) — **never log API keys**

### UI/UX quality
- Material 3 motion and typography; dark + gold brand consistent
- Loading / empty / error / success for every major screen
- Accessibility: content descriptions, touch targets, contrast
- Responsive to phones; tablet layouts later but don’t hardcode widths
- Onboarding must meet `docs/ONBOARDING.md` acceptance criteria

### Reliability
- Foreground service rules (types, notifications) respected for Android 14+
- Clean teardown of AudioRecord / sockets / players on Stop and process death
- Idempotent Start/Stop
- Crash-free path for denied permissions and invalid API keys

### Security & privacy
- Encrypted storage for secrets
- Secrets only in local.properties / CI — never git
- Privacy policy ready before ads / Play release
- Minimal permissions; justify each in Play Console

### Testing (ramp up)
- Unit tests for domain + key mappers
- UI tests for critical onboarding path when stable
- Manual checklist before any store build

### Performance
- Avoid main-thread I/O
- Baseline profiles later for cold start
- Dubbing path measured (latency stats) without blocking UI
