# PROJECT_CONTEXT — ParsLiveDub Android

Source of truth for AI assistants and developers. Update this file after every material change.

## 1. What this is

- **Name:** ParsLiveDub Android
- **Type:** Native Android application (separate from the Chrome extension)
- **Idea:** Real-time live dubbing of media (YouTube / system audio paths as implemented) using Google Gemini Live / related APIs
- **Repo:** https://github.com/mo3iiibest77-hub/ParsLiveDub-Android
- **Sister project (extension):** https://github.com/mo3iiibest77-hub/ParsLiveDub
- **Current phase:** 0 — Bootstrap / product definition (onboarding + monetization + quality specs)

## 2. Non-goals (for now)

- Do **not** rewrite the Chrome extension into Android
- Do **not** copy extension JS into the app
- Do **not** commit API keys or OAuth client secrets

## 3. Tech stack (mandatory)

- Kotlin 2.x
- Jetpack Compose + Material 3
- MVVM + unidirectional state
- Hilt
- Coroutines / Flow
- Media3
- DataStore
- Credential Manager + Google Sign-In
- OkHttp
- Foreground Service for active dubbing sessions
- Play Billing + AdMob hooks (see monetization doc)

## 4. Product pillars

1. **Auth:** Sign in with Google; session persisted securely
2. **Guided onboarding (HARD REQUIREMENT):** From first launch until first successful dub — `docs/ONBOARDING.md`
3. **API setup UX:** Non-technical Gemini key wizard + in-app test
4. **Dubbing engine:** Capture → model → playback; honest latency
5. **Localization:** Full UI i18n; user-selectable app language
6. **Dub target language:** Independent of UI language
7. **Background:** Foreground service + notification; Stop always available
8. **Revenue (HARD REQUIREMENT):** Freemium + ads + Premium subscription — designed from day one via entitlements. See `docs/MONETIZATION.md`
9. **Quality (HARD REQUIREMENT):** Production bar in `docs/QUALITY_BAR.md` — no prototype shortcuts in architecture

## 5. Inspiration from extension (lessons learned)

- Gemini Live latency ~2.5–3.5s is mostly model-side
- Never claim sub-1s with Live Translate alone
- Avoid crude per-chunk pitch shift noise
- Native Media3 can control video clock better than mobile Chrome
- Always clean up audio resources

## 6. Immediate next work

1. Gradle + Compose app shell at production structure (packages for ui / domain / data)
2. `EntitlementsRepository` stub (Free vs Premium) even before real ads
3. Onboarding navigation state machine
4. Theme dark + gold
5. EN + FA strings

## 7. AI continuation rules

- Read this file + ONBOARDING + MONETIZATION + QUALITY_BAR before major work
- Prefer small vertical slices that compile
- After each slice: update this file + commit with phase id
- Persian OK for user chat; code/commits in English
