# API Reference — Data Layer

## Product Entity

**Path:** `data/entity/Product.java`

Room entity representing a product in the database.

### Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | int | Auto-generated primary key |
| `nfcTagId` | String | NFC tag UID hex (unique index) |
| `name` | String | English product name |
| `nameAr` | String | Arabic product name (with tashkeel) |
| `brand` | String | English brand name |
| `brandAr` | String | Arabic brand name (with tashkeel) |
| `ingredients` | String | English ingredients |
| `ingredientsAr` | String | Arabic ingredients (with tashkeel) |
| `allergens` | String | Comma-separated: "dairy,nuts" |
| `allergensAr` | String | Arabic allergen labels |
| `weightGrams` | int | Package weight |
| `calories` | float | Per 100g |
| `protein` | float | Per 100g |
| `carbs` | float | Per 100g |
| `fat` | float | Per 100g |
| `isHalal` | boolean | Halal certified |
| `imageEmoji` | String | Display emoji |

### Localization Methods

All methods take a `lang` parameter ("en" or "ar"):

| Method | Returns |
|--------|---------|
| `getLocalizedName(lang)` | UI name (tashkeel stripped for Arabic) |
| `getLocalizedNameTts(lang)` | TTS name (tashkeel kept for Arabic pronunciation) |
| `getLocalizedBrand(lang)` | UI brand |
| `getLocalizedBrandTts(lang)` | TTS brand |
| `getLocalizedIngredients(lang)` | UI ingredients |
| `getLocalizedIngredientsTts(lang)` | TTS ingredients |
| `getLocalizedAllergens(lang)` | UI allergens |
| `getLocalizedAllergensTts(lang)` | TTS allergens |

---

## ScanHistory Entity

**Path:** `data/entity/ScanHistory.java`

Records each product scan with timestamp.

| Field | Type | Description |
|-------|------|-------------|
| `id` | int | Auto-generated PK |
| `productId` | int | FK → Product.id (CASCADE delete) |
| `scannedAt` | long | Timestamp (System.currentTimeMillis) |

---

## ProductDao

**Path:** `data/dao/ProductDao.java`

| Method | Return Type | Description |
|--------|-------------|-------------|
| `getProductByTagId(tagId)` | `LiveData<Product>` | Async lookup by NFC UID |
| `getProductByTagIdSync(tagId)` | `Product` | Sync lookup (background thread only) |
| `getProductById(id)` | `LiveData<Product>` | Lookup by primary key |
| `getProductByIdSync(id)` | `Product` | Sync version |
| `getAllProducts()` | `LiveData<List<Product>>` | All products |
| `searchByName(query)` | `LiveData<List<Product>>` | Search name/nameAr |
| `getProductCount()` | `int` | For seed guard |
| `insertAll(List<Product>)` | `void` | Bulk insert |

---

## ScanHistoryDao

**Path:** `data/dao/ScanHistoryDao.java`

| Method | Return Type | Description |
|--------|-------------|-------------|
| `insert(ScanHistory)` | `void` | Record a scan |
| `getAllHistory()` | `LiveData<List<ScanHistory>>` | All scans (newest first) |
| `getRecentScans(limit)` | `LiveData<List<ScanHistory>>` | Last N scans |
| `getScanCount()` | `LiveData<Integer>` | Total scan count |
| `getAllergenAlertCount()` | `int` | Sync count |
| `deleteAll()` | `void` | Clear history |

---

## AppDatabase

**Path:** `data/database/AppDatabase.java`

Room database singleton. Version 5 with destructive migration fallback.

### Seed Data

15 products pre-seeded in `onOpen` callback (not `onCreate`):

```java
private static final RoomDatabase.Callback seedDatabaseCallback = new RoomDatabase.Callback() {
    @Override
    public void onOpen(@NonNull SupportSQLiteDatabase db) {
        Executors.newSingleThreadExecutor().execute(() -> {
            ProductDao dao = INSTANCE.productDao();
            if (dao.getProductCount() > 0) return;  // Already seeded
            // Insert 15 products...
        });
    }
};
```

!!! note "Why onOpen instead of onCreate"
    `onCreate` doesn't fire after destructive migration. `onOpen` fires every time with a count guard, ensuring data is always present.

---

## ShoppingRepository

**Path:** `data/repository/ShoppingRepository.java`

Wraps DAOs and provides async execution via `ExecutorService`.

### Pattern

```java
public void getProductByTagIdSync(String tagId, ProductCallback callback) {
    executor.execute(() -> {
        Product product = productDao.getProductByTagIdSync(tagId);
        callback.onResult(product);
    });
}
```

### Callback Interfaces

- `ProductCallback` — `void onResult(Product product)`
- All write operations run on background executor
- Read operations return `LiveData` for UI observation
