# API Reference — UI Layer

## MainActivity

**Path:** `ui/MainActivity.java`

The single activity that hosts all fragments via Navigation Component.

### Key Responsibilities

- NFC foreground dispatch (enable/disable in onResume/onPause)
- Bottom navigation setup with fade transitions
- Theme application (system/light/dark)
- Font scale application
- Keyboard auto-dismiss on navigation

### Key Methods

| Method | Description |
|--------|-------------|
| `onCreate()` | Sets up navigation, bottom nav, NFC, handles launch intent |
| `onNewIntent(Intent)` | Receives NFC intents when app is in foreground |
| `handleIntent(Intent)` | Extracts NFC tag ID, triggers product lookup via ViewModel |
| `setupNfc()` | Initializes NfcAdapter and PendingIntent for foreground dispatch |
| `applyFontScale()` | Reads font size from prefs, applies to Configuration |
| `showNfcDisabledDialog()` | Prompts user to enable NFC in system settings |

### NFC Flow

```java
onNewIntent(intent)
  → setIntent(intent)
  → handleIntent(intent)
    → NfcHelper.getTagId(tag)  // extract UID hex
    → homeViewModel.lookupProductByTagId(tagId)
    // Sound + vibration handled in HomeFragment observers
```

---

## SplashFragment

**Path:** `ui/splash/SplashFragment.java`

Animated splash screen with icon bounce, circle expand, ring ripple, title slide-up.

### Key Behavior

- AnimatorSet with 4 animation phases
- Tap anywhere to skip
- Auto-navigate after 4.5 seconds
- **Skips entirely if launched via NFC intent** (background scan)
- `hasNavigated` flag prevents double navigation

---

## WelcomeFragment

**Path:** `ui/welcome/WelcomeFragment.java`

Onboarding screen shown on first launch only.

### Collects

- User name (required — button disabled until entered)
- Dietary restrictions (chip toggles)
- Language preference (English/Arabic — switches immediately)

### Key Logic

```java
if (!prefs.isFirstLaunch()) {
    // Skip to home
    Navigation.findNavController(view).navigate(R.id.action_welcome_to_home);
    return;
}
```

---

## HomeFragment

**Path:** `ui/home/HomeFragment.java`

Main screen with scan area, search bar, recent scans, tutorial.

### Key Methods

| Method | Description |
|--------|-------------|
| `onViewCreated()` | Binds views, sets greeting, observers, tutorial |
| `playScanSound(boolean)` | Plays success chime or error tone + vibration |
| `showSimulateDialog()` | Shows product picker for emulator testing |
| `startNfcPulse()` | Starts pulsing animation on NFC icon |

### Observers

- `getScannedProduct()` → navigate to ProductDetail + success sound
- `getScanError()` → snackbar + error sound
- `getRecentScans()` → update RecyclerView
- `getSearchNotFound()` → snackbar

---

## ProductDetailFragment

**Path:** `ui/product/ProductDetailFragment.java`

Full product information with TTS and AI recommendations.

### Key Methods

| Method | Description |
|--------|-------------|
| `speakProductInfo(Product)` | Chains TTS: name → nutrition → ingredients → allergens → recs |
| `speakRecommendationsOnly(List)` | Speaks just recommendations (called when they load late) |
| `showRecommendationBottomSheet(Recommendation)` | Full nutrition bottom sheet + auto-speak |
| `autoReadProduct()` | Triggered after allergen warning finishes |

### TTS Chain

```java
speakThenRun(name, () ->
  speakThenRun(nutrition, () ->
    speakThenRun(allergens, () ->
      speakRecommendationsOnly(recs)
    )
  )
)
```

### Flags

| Flag | Purpose |
|------|---------|
| `hasAutoRead` | Prevents double auto-read on same product |
| `productSpoken` | Tracks if product info was spoken |
| `recsSpoken` | Tracks if recommendations were spoken |

---

## HistoryFragment

**Path:** `ui/history/HistoryFragment.java`

Scan history with filter chips (All, Allergens, Safe, Today).

### Key Features

- Filter chips with `selectionRequired="true"`
- Clear history with confirmation dialog
- Background thread loads product data for each scan entry
- `isAdded()` + `getView().post()` guards for thread safety

---

## SettingsFragment

**Path:** `ui/settings/SettingsFragment.java`

All app preferences in one screen.

### Settings

| Setting | Type | Values |
|---------|------|--------|
| Name | TextInput | Free text |
| Dietary Profile | Filter Chips | dairy-free, gluten-free, low-sodium, vegan, nut-allergy |
| Theme | Chips | System / Light / Dark |
| Font Size | Chips | Small / Medium / Large |
| Speech Speed | Chips | x0.5 / x1 / x1.5 |
| Auto TTS | Switch | On / Off |
| AI Recommendations | Switch | On / Off |
| Reset Tutorial | Button | Resets all tutorial flags |
| Reset Defaults | Button | Clears all prefs (with confirmation) |

---

## Adapters

### RecentScanAdapter

**Path:** `ui/adapter/RecentScanAdapter.java`

Displays last 5 scanned products on home screen. Uses background executor + `view.post()` for sync product lookup.

### HistoryAdapter

**Path:** `ui/adapter/HistoryAdapter.java`

Full history list with allergen/safe badges. Inner `HistoryItem` class combines `ScanHistory` + `Product`.

### RecommendationAdapter

**Path:** `ui/adapter/RecommendationAdapter.java`

AI recommendation cards. Shows English name, Arabic name (if available), reason, match percentage bar. Detects language for display.
