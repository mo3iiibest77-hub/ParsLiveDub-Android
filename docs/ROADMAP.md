# Roadmap — ParsLiveDub Android

## Phase 0 — Bootstrap ✅
- [x] GitHub repository
- [x] README + PROJECT_CONTEXT + architecture notes

## Phase 1 — App shell
- [ ] Gradle project (`app` module), Compose Material 3 theme
- [ ] Navigation: Splash, Onboarding, Sign-In, Home, Settings
- [ ] App language switcher (EN + FA minimum)
- [ ] Secure storage scaffold for tokens/settings (DataStore)

## Phase 2 — Identity & onboarding
- [ ] Google Sign-In (Credential Manager)
- [ ] Permission flows: microphone / notification / media projection as needed
- [ ] Gemini API key onboarding wizard (copy steps + open AI Studio)
- [ ] “First success” checklist UI

## Phase 3 — Core dubbing MVP
- [ ] Audio capture pipeline
- [ ] Gemini Live (or approved) WebSocket/client integration
- [ ] Playback of translated audio + ducking original when applicable
- [ ] Foreground service + notification controls (Start/Stop)
- [ ] Target language picker

## Phase 4 — YouTube / in-app player
- [ ] In-app playback path where Media3 can delay **video** vs audio intentionally
- [ ] Sync controls (offset slider)
- [ ] Robust error recovery

## Phase 5 — Polish & commerce prep
- [ ] Full i18n pass (many locales)
- [ ] Crash-free quality bar, analytics (optional, privacy-first)
- [ ] Play Store listing assets
- [ ] Optional: Play Billing for Pro

## Out of scope until requested
- Full offline translation models
- iOS
