# Monetization strategy — ParsLiveDub Android

Revenue is a **first-class product requirement**, designed in from day one — not bolted on after launch.

## Model: Freemium + Ads + Optional subscription

```
Free tier          → real value, limited
  + Ads            → AdMob (or Play-compliant network)
  + Rewarded ads   → optional extra minutes / features
Premium (sub)      → remove ads + higher limits + pro features
(Future)           → one-time lifetime / team plans
```

## Free tier (acquisition)

Suggested defaults (tunable):
- Guided onboarding fully available
- Daily dubbing time cap **or** session length cap
- Standard voice / standard model path
- Ads: banner on Home/Settings **or** interstitial between sessions (never mid-sentence during Live)
- Optional rewarded ad: “+15 minutes today”

## Premium (Play Billing)

Subscription (monthly / yearly):
- **No ads**
- Higher or unlimited daily quota
- Priority / better model route if product offers it
- Picture sync controls, history, export — as features ship
- Early access flags

Implement with **Play Billing Library 6+**, product IDs in a single `BillingCatalog` object, and a `EntitlementsRepository` so UI never talks to Billing APIs directly.

## Ads — rules (quality & trust)

| Do | Don’t |
|----|--------|
| Load ads only after onboarding basics | Show ads during active Live dub audio path |
| Clear “Remove ads” path to Premium | Accidental clicks, fake close buttons |
| Use test ads in debug builds | Ship test ad unit IDs to production |
| Respect TMP / child flags if applicable | Ads on permission or API-key entry screens |
| Frequency cap interstitials | Full-screen ad every Start press |

**Recommended placements**
1. Home screen — small banner (free only)
2. After Stop session — optional interstitial (capped)
3. Rewarded — user-initiated “Watch ad for more time”

## Architecture hooks (build early)

Even before real ad IDs:

```
domain/entitlements/
  UserPlan: Free | Premium
  canStartSession(): Boolean
  adsEnabled(): Boolean

data/billing/
  BillingRepository (Play Billing)

data/ads/
  AdsRepository (AdMob) — no-op or test in debug

ui/
  observes UserPlan; hides banners if Premium
```

Feature flags via DataStore / Remote Config later:
- `ads_enabled`
- `free_daily_minutes`
- `premium_sku_monthly`

## Compliance

- Google Play Payments Policy for digital features (subs via Play Billing)
- Privacy Policy + Data safety form before ads
- UMP / consent for EEA if serving ads
- Accurate store listing (what Free vs Premium includes)

## Metrics that matter

- Install → onboarding complete → first successful dub
- Free → rewarded opt-in rate
- Free → Premium conversion
- Ad ARPDAU vs churn after interstitial
- Quota hit rate (if free is too tight, conversion may rise but reviews fall)

## Phased delivery

| Phase | Monetization work |
|-------|-------------------|
| 1–2 | Entitlements interface + Free plan defaults (no real ads yet) |
| 3 | Wire session limits to entitlements |
| 4 | AdMob test units + placements behind `adsEnabled()` |
| 5 | Play Billing Premium + remove-ads |
| 6 | Tune caps, creatives, paywall copy from real data |

## Paywall UX principles

- Show value before ask (after first success, not before API setup)
- One clear CTA: Start Premium
- Restore purchases button
- Transparent comparison Free vs Premium
