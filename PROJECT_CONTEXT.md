# PROJECT_CONTEXT — ParsLiveDub Android

Source of truth for AI assistants and developers. Update this file after every material change.

## 1. What this is

- **Name:** ParsLiveDub Android
- **Type:** Native Android application (separate from the Chrome extension)
- **Idea:** Real-time live dubbing of media (YouTube / system audio paths as implemented) using Google Gemini Live / related APIs
- **Repo:** https://github.com/mo3iiibest77-hub/ParsLiveDub-Android
- **Sister project (extension):** https://github.com/mo3iiibest77-hub/ParsLiveDub
- **Current phase:** 0 — Bootstrap / product definition

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

## 4. Product pillars

1. **Auth:** Sign in with Google; session persisted securely
2. **Onboarding:** Step-by-step from install → permissions → API setup → first dub
3. **API setup UX:** Guide non-technical users to Gemini API key / Google AI Studio (in-app browser or clear external steps)
4. **Dubbing engine:** Capture → stream to model → play translated audio; measure and improve latency
5. **Localization:** Entire UI localizable; user can change app language in Settings
6. **Dub target language:** Independent of UI language (reuse extension language list as a starting point)
7. **Background:** User-visible notification while session is live; stop is always one tap away
8. **Commercial path:** Architecture must allow premium features later (Play Billing later phase)

## 5. Inspiration from extension (lessons learned)

- Gemini Live latency ~2.5–3.5s is mostly model-side; client buffers only
- Never claim sub-1s with Live Translate alone
- Pitch-shift per short chunk caused noise — prefer model voice quality over crude WSOLA on mobile
- Browser mobile could not reliably lag video frames; **native Media3 can control video clock** for true picture lag when playing in-app
- Always clean up audio resources; never leave ducked/muted state

## 6. Immediate next work

1. Android Studio project skeleton (Gradle Kotlin DSL, `app` module, Compose empty shell)
2. Theme (dark + gold brand direction from extension UI)
3. Navigation graph: Splash → Onboarding → Auth → Home → Settings
4. Google Sign-In wiring (debug SHA-1 documented for the owner)
5. String resources: English + Persian first, then expand

## 7. AI continuation rules

- Read this file before coding
- Prefer small vertical slices that compile
- After each slice: update this file + commit message with phase id
- Persian user communication OK; code/comments/commits in English
