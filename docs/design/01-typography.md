# LyraApp — Tipografi Sistemi

> Bu dosya LyraApp isimli uygulamanın tipografi yapısı için
> **tek doğruluk kaynağıdır** (single source of truth) ve
> doğrudan bir **Android Jetpack Compose** projesinde kullanılmak
> üzere düzenlenmiştir.

---

## 1. Temel Kurallar

> Hiçbir `@Composable` içinde ham `TextStyle(...)` yazılmaz.
> Tipler daima `MaterialTheme.typography.<slot>` üzerinden
> okunmak zorundadır.
>
> Ham `TextStyle(...)` tanımı yalnızca `Type.kt` içinde,
> `LyraTypography` nesnesi tanımlanırken kullanılır.

---

## 2. Font Ailesi

**Roboto** — Google tarafından tasarlanmış, Material Design ile uyumlu sans-serif font.
Android sisteminde varsayılan olarak bulunur; ek bağımlılık gerekmez.

| Ağırlık        | Değer | Kullanım Alanı                      |
|----------------|-------|--------------------------------------|
| Roboto Regular | 400   | Body metinleri, genel içerik         |
| Roboto Medium  | 500   | Title stilleri, buton etiketleri     |
| Roboto Bold    | 700   | Ekran başlıkları, şarkı adı          |

---

## 3. `Type.kt` — Tipografi Token Tanımları

| Slot           | Weight  | Size  | LineHeight | LetterSpacing | LyraApp Ekranı & Kullanımı                              |
|----------------|---------|-------|------------|---------------|---------------------------------------------------------|
| headlineLarge  | Bold    | 32sp  | 40sp       | 0sp           | **Ana Sayfa** — "Ne dinlemek istersin?"                 |
| headlineMedium | Bold    | 28sp  | 36sp       | 0sp           | **Now Playing** — şarkı adı             |
| headlineSmall  | Regular | 24sp  | 32sp       | 0sp           | **Playlist Detay** — playlist adı        |
| titleLarge     | Medium  | 22sp  | 28sp       | 0sp           | **Kütüphane / Arama** — ekran başlığı                   |
| titleMedium    | Medium  | 16sp  | 24sp       | 0.15sp        | **Kütüphane listesi** — playlist adı satırı             |
| titleSmall     | Medium  | 14sp  | 20sp       | 0.1sp         | **Playlist Detay** — şarkı listesi öğesi adı            |
| bodyLarge      | Regular | 16sp  | 24sp       | 0.5sp         | **Now Playing** — sanatçı / albüm adı  |
| bodyMedium     | Regular | 14sp  | 20sp       | 0.25sp        | **Playlist Detay** — açıklama metni                     |
| bodySmall      | Regular | 12sp  | 16sp       | 0.4sp         | **Kütüphane / Playlist** — meta bilgi |
| labelLarge     | Medium  | 14sp  | 20sp       | 0.1sp         | **Butonlar** — Çal, Kaydet                              |
| labelMedium    | Medium  | 12sp  | 16sp       | 0.5sp         | **BottomNav** — Ana sayfa, Ara, Kütüphane…              |
| labelSmall     | Medium  | 11sp  | 16sp       | 1.5sp         | **Now Playing** — "ŞİMDİ ÇALIYOR" (all-caps üst etiket)|

> **Not:** `labelSmall` için `letterSpacing = 1.5sp` all-caps görünümünü desteklemek
> amacıyla artırılmıştır. `toUpperCase(Locale.getDefault())` ile birlikte kullanılır.

---

## 4. Ekran Bazlı Kullanım Haritası

### Ana Sayfa
| Öğe                        | Slot            |
|----------------------------|-----------------|
| Selamlama (saate göre dinamik)            | `labelMedium`   |
| "Ne dinlemek istersin?"    | `headlineLarge` |
| Hızlı seçim kart adı       | `titleSmall`    |
| "Son çalınanlar" başlığı   | `titleMedium`   |
| Şarkı adı (mini kart)      | `titleSmall`    |
| Sanatçı adı (mini kart)    | `bodySmall`     |

### Now Playing
| Öğe                        | Slot             |
|----------------------------|------------------|
| "ŞİMDİ ÇALIYOR"            | `labelSmall`     |
| Playlist adı               | `titleMedium`    |
| Şarkı adı                  | `headlineMedium` |
| Sanatçı / Albüm adı        | `bodyLarge`      |
| Süre          | `labelMedium`    |

### Playlist Detay
| Öğe                         | Slot            |
|-----------------------------|-----------------|
| Playlist adı                | `headlineSmall` |
| Açıklama                    | `bodyMedium`    |
| Meta (sanatçı · şarkı · dk) | `bodySmall`     |
| Şarkı adı (liste satırı)    | `titleSmall`    |
| Sanatçı adı (liste satırı)  | `bodySmall`     |
| Süre                        | `labelMedium`   |

### Kütüphane
| Öğe                        | Slot            |
|----------------------------|-----------------|
| "Kütüphane" başlığı        | `titleLarge`    |
| Playlist adı (liste)       | `titleMedium`   |
| Alt bilgi (tür · şarkı)    | `bodySmall`     |

### BottomNavigationBar
| Öğe                        | Slot            |
|----------------------------|-----------------|
| Sekme etiketi              | `labelMedium`   |

---

## 5. Kullanım Örnekleri

```kotlin
// Now Playing — şarkı adı
Text(
    text = song.title,
    style = MaterialTheme.typography.headlineMedium
)

// Now Playing — sanatçı adı
Text(
    text = song.artist,
    style = MaterialTheme.typography.bodyLarge,
    color = MaterialTheme.colorScheme.onSurfaceVariant
)

// Now Playing — "ŞİMDİ ÇALIYOR" etiketi
Text(
    text = "ŞİMDİ ÇALIYOR",
    style = MaterialTheme.typography.labelSmall,
    color = MaterialTheme.colorScheme.onSurfaceVariant
)

// Playlist meta bilgisi
Text(
    text = "${playlist.songCount} şarkı · ${playlist.duration} dk",
    style = MaterialTheme.typography.bodySmall,
    color = MaterialTheme.colorScheme.onSurfaceVariant
)

// BottomNav etiketi
Text(
    text = label,
    style = MaterialTheme.typography.labelMedium
)
```

---

## 6. Erişilebilirlik

- Minimum font boyutu: **11sp** (`labelSmall`)
- `fontScale` sistemi desteklenir; sabit `px` kullanılmaz
- Kontrast: WCAG AA — minimum 4.5:1 oranı
- All-caps metin yalnızca `labelSmall` slotunda kullanılır

