# API Reference — ViewModels

## HomeViewModel

**Path:** `model/HomeViewModel.java`

Shared ViewModel (activity-scoped) for NFC scan results and product search.

### LiveData

| Field | Type | Description |
|-------|------|-------------|
| `scannedProduct` | `MutableLiveData<Product>` | Set when NFC tag matches a product |
| `scanError` | `MutableLiveData<String>` | Set when NFC tag has no matching product (contains tag ID) |
| `recentScans` | `LiveData<List<ScanHistory>>` | Last 5 scans for home screen |
| `searchNotFound` | `MutableLiveData<Boolean>` | Set when text search finds nothing |

### Key Methods

| Method | Description |
|--------|-------------|
| `lookupProductByTagId(String tagId)` | Async lookup by NFC UID. Posts to `scannedProduct` or `scanError` |
| `searchProductByName(String query)` | Search products by name, navigate on match |
| `clearScannedProduct()` | Resets LiveData after navigation |
| `clearScanError()` | Resets error after snackbar shown |
| `getProductByIdSync(int id)` | Sync lookup for adapters (background thread only) |

---

## HistoryViewModel

**Path:** `model/HistoryViewModel.java`

Manages scan history queries.

### LiveData

| Field | Type | Description |
|-------|------|-------------|
| `allHistory` | `LiveData<List<ScanHistory>>` | All scans, newest first |

### Key Methods

| Method | Description |
|--------|-------------|
| `getAllHistory()` | Returns observed history list |
| `getProductByIdSync(int id)` | Sync product lookup for history items |
| `clearHistory()` | Deletes all scan records |

---

## ProductViewModel

**Path:** `model/ProductViewModel.java`

Manages product details and AI recommendations.

### LiveData

| Field | Type | Description |
|-------|------|-------------|
| `recommendations` | `MutableLiveData<List<Recommendation>>` | AI recommendation results |
| `aiError` | `MutableLiveData<String>` | Error message if Groq fails |
| `isLoadingAi` | `MutableLiveData<Boolean>` | Loading state for progress bar |

### Key Methods

| Method | Description |
|--------|-------------|
| `fetchRecommendations(Product, Context)` | Triggers AI recommendation pipeline |
| `getRecommendations()` | Returns observed recommendation list |
| `getAiError()` | Returns observed error |
| `getIsLoadingAi()` | Returns observed loading state |
| `hasAllergenConflict(String allergens)` | Checks if product allergens conflict with user dietary profile |
| `searchProductByName(String, Callback)` | Search for recommendation product in DB |

### Recommendation Flow

```java
fetchRecommendations(product, context)
  → Check if AI is enabled
  → Set isLoadingAi = true
  → AiRecommendationManager.getRecommendations(
        product.name,
        product.ingredients,
        product.allergens,
        userProfile,
        product.calories,
        product.fat,
        callback
    )
  → onSuccess: recommendations.postValue(recs)
  → onError: aiError.postValue(message)
  → Always: isLoadingAi.postValue(false)
```
