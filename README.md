# Crypto & Weather — Kotlin Android Demo

[English](#overview) | [Türkçe](#türkçe)

## Overview

An Android learning project demonstrating Retrofit API integration with Kotlin and Jetpack Compose. It combines a searchable cryptocurrency sample list with a city-based weather screen.

**The cryptocurrency data is a static JSON sample, not a live market feed.** Card colors represent fixed price thresholds, not price movements or investment signals.

## Features

- Retrieve cryptocurrency sample records and display them in a LazyColumn.
- Filter currency names with case-insensitive text search.
- Show loading, error, and empty-result states on the home screen.
- Navigate between home and weather screens using Navigation Compose.
- Request weather by city name (initial input: Edirne).
- Display a weather description and temperature in Celsius.
- Deserialize JSON responses into Kotlin data classes with Gson.

Humidity is present in the weather response model but is not displayed by the inspected screen.

## Technology Stack

| Component | Repository configuration |
| --- | --- |
| Kotlin | 2.0.0 |
| Jetpack Compose | BOM 2024.04.01; Material 3 |
| Navigation Compose | 2.8.9 |
| Retrofit / Gson converter | 2.11.0 |
| Kotlin coroutines Android | 1.3.9 |
| Android Gradle Plugin | 8.7.2 |
| Gradle wrapper | 8.9 |
| Minimum Android SDK | 24 |
| Compile / target SDK | 35 |
| Java source / Kotlin JVM target | 11 |

These are checked-in versions, not recommendations to use the latest releases. The Java compilation target does not by itself specify the JDK needed to run Gradle.

## Source Guide

Paths below are relative to `app/src/main/java/com/example/refrodit/`.

| File | Responsibility |
| --- | --- |
| `View/MainActivity.kt` | Navigation, cryptocurrency loading, search, and cards |
| `View/WeatherDetailsScreen.kt` | Weather input, request, and result UI |
| `Service/CryptoAPI.kt` | Retrofit interface for the sample JSON |
| `Service/WeatherAPI.kt` | Retrofit weather request with city, API key, and metric units |
| `Model/CryptoModel.kt` | Cryptocurrency response model |
| `Model/WeatherResponse.kt` | Weather response models |
| `ui/theme/` | Compose theme configuration |

The existing package/application identifier `com.example.refrodit` is retained. Some weather files have no package declaration.

## Data Sources

- Cryptocurrency sample: `atilsamancioglu/K21-JSONDataSet`, file `crypto.json`, accessed through GitHub's raw-content host.
- Weather: OpenWeather endpoint `/data/2.5/weather`, with `q`, `appid`, and `units=metric` query parameters.

No external requests were made during this documentation update. Endpoint availability and returned data were not verified.

## API Key Safety

The weather screen contains a hardcoded API key. **Treat it as exposed and revoke or rotate it through its provider if it is active.** Its value is intentionally not reproduced here.

Before running the weather feature, replace the embedded credential with your own local development configuration and update the code to read it. No configurable key-loading mechanism is currently implemented.

Keeping a key out of Git reduces accidental disclosure, but packaging it in an Android app does not make it secret. Use an appropriate server-side integration when the credential must remain confidential. Removing a key from the latest source alone does not remove it from Git history.

## Local Setup

1. Clone the repository using its current URL from GitHub's **Code** menu.
2. Open the repository root in Android Studio.
3. Install Android SDK 35 and use a Gradle JDK compatible with the included Android Gradle Plugin.
4. Allow Gradle sync and dependency resolution.
5. Complete the API-key safety steps above before requesting weather. Do not use the checked-in credential.
6. Select an emulator or device with API level 24 or later.
7. Run the `app` configuration.

Internet access is required. No offline cache is implemented.

### Build and Test Commands

On Windows, from the repository root:

```powershell
.\gradlew.bat assembleDebug
.\gradlew.bat testDebugUnitTest
```

On macOS/Linux:

```bash
bash gradlew assembleDebug
bash gradlew testDebugUnitTest
```

The included tests check basic arithmetic and the application package name; they do not verify API responses, search, weather behavior, or failure states. No build or test command was executed during this documentation update.

## Known Limitations

- Weather requests do not catch network exceptions; a transport failure can interrupt the request flow instead of displaying the intended error.
- A successful cryptocurrency response with a null body does not call either result callback, potentially leaving the loading state active.
- Cryptocurrency loading uses a manually created coroutine scope; the stored job is not cancelled in an activity lifecycle callback.
- Weather Retrofit configuration is rebuilt for each request.
- Weather results are labeled with the editable city input rather than the returned city name, so changing the input can mislabel an existing result.
- No persistent cache, refresh policy, or application-specific automated tests are included.
- Retrofit is declared twice in the dependency block, and the Internet permission appears twice in the manifest.

Only documentation was added; application code and credentials were not changed.

## Türkçe

### Proje Hakkında

Kotlin, Jetpack Compose ve Retrofit ile API kullanımını gösteren Android eğitim projesidir. Aranabilir bir kripto para örnek listesi ile şehir adına göre hava durumu sorgulama ekranını birleştirir.

**Kripto verileri canlı piyasa fiyatları değildir; sabit bir JSON örnek dosyasından gelir.** Kart renkleri fiyat artışı/azalışını değil, kodda belirlenen sabit eşikleri gösterir.

### Özellikler

- Kripto para örnek kayıtlarını listeleme.
- Para birimi adına göre büyük/küçük harf duyarsız arama.
- Ana ekranda yüklenme, hata ve boş sonuç durumları.
- Navigation Compose ile ekran geçişleri.
- Şehir adına göre hava durumu sorgulama; başlangıç değeri Edirne.
- Hava durumu açıklaması ve Celsius sıcaklık gösterimi.
- Gson ile JSON verilerini Kotlin modellerine dönüştürme.

### Teknolojiler ve Kurulum

Kotlin 2.0.0, Jetpack Compose, Material 3, Retrofit 2.11.0 ve coroutines kullanılır. Minimum SDK 24, compile/target SDK 35'tir. Gradle wrapper 8.9 ve Android Gradle Plugin 8.7.2 projede tanımlıdır.

1. Depoyu klonlayıp kök klasörü Android Studio ile açın.
2. Android SDK 35'i ve Gradle eklentisiyle uyumlu JDK ortamını hazırlayın.
3. Gradle senkronizasyonunu tamamlayın.
4. Hava durumu özelliğini çalıştırmadan önce aşağıdaki API anahtarı uyarısını dikkate alın.
5. API 24 veya üzeri cihaz/simülatör seçip `app` yapılandırmasını çalıştırın.

### API Anahtarı Uyarısı

Hava durumu ekranında sabit bir API anahtarı bulunmaktadır. Aktifse sağlayıcı üzerinden iptal edilmeli veya yenilenmelidir; anahtar burada tekrar paylaşılmamıştır.

Kendi geliştirme anahtarınızı Git'e eklenmeyen yerel yapılandırmadan okuyacak düzenleme yapılmalıdır. Mevcut projede böyle bir yapılandırma mekanizması yoktur. Anahtarı APK içine eklemek onu gizli tutmaz; gizli kalması gereken bilgiler için sunucu tarafı çözüm gerekir.

### Mevcut Durum

Hava durumu isteğinde ağ istisnaları yakalanmıyor. Kripto yanıtının gövdesi boş geldiğinde yüklenme durumu açık kalabilir. Coroutine yaşam döngüsü, sonuçların şehir etiketi ve hata yönetimi geliştirilmelidir.

Testler yalnızca basit aritmetik ve paket adı kontrolünden oluşur. Bu güncellemede README eklenmiş; kod veya anahtar değiştirilmemiş, uygulama derlenmemiş, testler ve harici API istekleri çalıştırılmamıştır.
