# Onboarding & Guided Journey

**Goal:** From the first second after install until the user successfully plays a video with live dubbing, the app never leaves them alone. Every blocker has a screen, a reason, and a next action.

This is a **product requirement**, not optional polish.

---

## Principles

1. **One primary action per screen** (Next / Allow / Open link / Start).
2. **Progress is visible** (step indicator: e.g. 2 of 6).
3. **Skip only when safe** (never skip mic permission if dubbing needs it).
4. **State is persisted** (DataStore): user can kill the app and resume the same step.
5. **UI language** follows system or user choice; all onboarding strings are localizable.
6. **No raw jargon without explanation** — “API key” is explained in plain language.
7. **Never store secrets in git**; keys go to encrypted preferences only.

---

## Journey map (happy path)

```
Install
  → Splash (route by saved progress)
  → Welcome
  → Choose app language (optional if system OK)
  → Google Sign-In
  → Why permissions (education)
  → Runtime permissions (mic / notifications / media as needed)
  → Gemini API setup wizard
  → Target dub language
  → How it works (15s tips)
  → Pick source (YouTube link / tab-style capture path)
  → First run checklist
  → Live session
  → Success celebration + “You’re ready”
```

If any step fails → dedicated recovery screen, not a toast-only error.

---

## Step catalog

### Step 0 — Splash
- Read DataStore: `onboardingCompleted`, `lastStep`, auth session.
- Route:
  - No progress → Welcome
  - Mid-onboarding → resume `lastStep`
  - Completed + signed in → Home
  - Completed + signed out → Sign-In

### Step 1 — Welcome
- Value prop: “Live dub any video into your language.”
- CTA: Get started
- Secondary: Already have an account → Sign-In

### Step 2 — App language
- List of UI locales (EN, FA, AR, … expandable).
- Apply `AppCompatDelegate.setApplicationLocales` (or equivalent).
- CTA: Continue

### Step 3 — Google Sign-In
- Button: Continue with Google (Credential Manager).
- Explain: used for account, sync preferences later, not for reading YouTube private data without consent.
- Errors: cancelled, network, Play Services missing → recovery copy + retry.

### Step 4 — Permissions education
- Cards:
  - Microphone / audio capture — “Needed to hear the video and dub it.”
  - Notifications — “So you can control dubbing when the screen is off.”
  - (If used) Media projection / nearby media — plain language.
- CTA: Continue to allow

### Step 5 — Permission requests
- Request in logical order; after each denial, show **why** + “Open settings” vs “Try again”.
- Do not mark onboarding complete if hard-required permission denied.

### Step 6 — Gemini API wizard (critical)
Sub-steps inside one flow (`ApiSetupGraph`):

| Sub | UI | Action |
|-----|----|--------|
| 6.1 | What is an API key? | Short plain explanation + illustration |
| 6.2 | Create key | Button **Open Google AI Studio** (Custom Tab / browser) → https://aistudio.google.com/apikey |
| 6.3 | Copy key | Instructions: create → copy |
| 6.4 | Paste key | Secure text field + show/hide; validate non-empty format |
| 6.5 | Test connection | Call lightweight Gemini check; success / invalid / quota errors |
| 6.6 | Saved | Confirm stored encrypted; CTA Continue |

Rules:
- Paste field never screenshots into logs.
- “Test connection” is mandatory before Continue (or warn strongly).
- Link “Change key later” in Settings.

### Step 7 — Dub target language
- Searchable list (70+), default by device locale if available.
- Independent from **app UI** language.
- CTA: Continue

### Step 8 — How it works
- 3 short cards: Select video → Start dub → Listen (latency honesty: ~a few seconds is normal).
- CTA: Try with a video

### Step 9 — First video
Options (implement what product prioritizes):
- A) Paste YouTube URL → in-app player
- B) Guided “open YouTube / pick audio source” if using projection/capture
- Always show pre-flight checklist:
  - [x] Signed in
  - [x] API key OK
  - [x] Permissions OK
  - [ ] Video playing / source active

### Step 10 — First Live session
- Big **Start dubbing** button.
- States: Connecting → Live → Error(recoverable).
- Foreground notification when Live.
- If success for N seconds → mark `onboardingCompleted = true`.

### Step 11 — Success
- “All set” screen: shortcuts to Home, Settings, change language, change API key.
- Optional: rate / tip about headphones.

---

## State machine (DataStore keys)

```
onboarding.completed: Boolean
onboarding.step: String enum
user.uiLocale: String
user.dubLanguage: String
auth.googleId: String?
gemini.hasKey: Boolean   // not the key itself in plain prefs if avoidable
gemini.keyValidatedAt: Long
permissions.micGranted: Boolean
permissions.notifGranted: Boolean
```

Encrypted storage for the actual API key.

---

## Empty / error / blocked UX

| Situation | Screen behavior |
|-----------|-----------------|
| No network | Retry + offline explanation |
| Invalid API key | Back to paste + AI Studio link |
| Quota 429 | Wait / new key / plain explanation |
| Mic denied forever | Open app settings deep link |
| Google Sign-In fail | Retry + “continue limited?” only if product allows guest mode |
| Session drops | Reconnect CTA without restarting whole onboarding |

---

## Analytics (optional, privacy-first)

Event names only, no key material:
- `onboarding_started`
- `onboarding_step_reached`
- `api_test_success` / `api_test_fail`
- `first_dub_success`
- `onboarding_completed`

---

## Acceptance criteria

A new user on a clean install can:
1. Understand the product
2. Sign in with Google
3. Grant required permissions
4. Obtain and validate a Gemini API key **inside the guided flow**
5. Choose UI language and dub language
6. Start a first video session and hear dubbed audio
7. Return later without seeing Welcome again

If any step is missing, onboarding is incomplete.
