# Plak Rafı — Android Müzik Çalar

Java ile yazılmış, eski Play Müzik tarzında basit bir müzik çalar. Cihazdaki
müzikleri kendisi tarar (MediaStore), albüm kapaklarını gösterir, parçaya
dokununca çalar ve arka planda (foreground servis + bildirim) çalmaya devam eder.

- **minSdk:** 21 (Android 5.0)
- **targetSdk / compileSdk:** 34 (Android 16 çıktığında Android Studio'da
  `compileSdk`/`targetSdk` değerini 36'ya yükseltmeniz yeterli — kod zaten
  `Build.VERSION.SDK_INT` kontrolleriyle her sürüme göre izin/servis
  davranışını ayarlıyor)
- **Dil:** Java

## Kurulum (Android Studio)

Bu klasörde `gradlew` dosyaları yok (ikili dosya olduğu için burada
oluşturulamadı), o yüzden en kolay yol:

1. Android Studio'da **File → Open** ile bu `PlakRafiAndroid` klasörünü açın.
2. Android Studio, gradle wrapper'ı olmadığını görüp otomatik olarak
   kendi bundled Gradle sürümüyle senkronize etmeyi teklif edecek —
   "Sync Now" / "Trust Project" seçeneklerini onaylayın.
   - Sorun yaşarsanız alternatif: Android Studio'da **File → New → New Project →
     Empty Views Activity**, paket adını `com.plakrafi.musicplayer`, dili
     **Java**, minimum SDK'yı **API 21** seçerek yeni bir proje oluşturun,
     ardından bu klasördeki `app/src` içeriğini yeni projenin `app/src`
     klasörünün üzerine kopyalayın ve `app/build.gradle` içindeki
     `dependencies` bloğunu buradakiyle birleştirin.
3. Gerçek bir cihaza veya emülatöre çalıştırın (emülatörde test için
   emülatörün içine birkaç mp3 dosyası atmanız gerekir).

## Neler var

- `MainActivity` — izin isteme + albüm ızgarası (kapak, isim, parça sayısı)
- `AlbumSongsActivity` — albümdeki parça listesi, dokununca çalma
- `PlayerActivity` — büyük kapak, ilerleme çubuğu, önceki/oynat-duraklat/sonraki
- `MusicService` — arka planda çalma; bildirim üzerinden kontrol; kilit
  ekranı / kulaklık düğmeleri için `MediaSessionCompat`; kulaklık çıkarılınca
  otomatik duraklatma; ses odağı (audio focus) yönetimi
- `MusicRepository` — `MediaStore` üzerinden albüm/parça tarama
- `AlbumArtLoader` — kapak resmini önce hızlı yoldan, olmazsa dosyaya gömülü
  kapaktan çıkarıp önbellekler

## Bilmeniz gereken sınırlamalar

- Bunu bu ortamda derleyip cihazda test edemedim (Android SDK/emülatör burada
  yok); yapı standart bir Android Studio projesi olduğu için sorunsuz
  senkronize olması beklenir, ama Android Studio'da ilk build'de küçük bir
  uyarı çıkarsa (ör. bir Gradle/AGP sürüm uyumsuzluğu) Android Studio'nun
  önerdiği "Upgrade" butonuna basmanız yeterli olur.
- Uygulama simgesi basit bir vektör; isterseniz Android Studio'daki
  **Image Asset** aracıyla kendi ikonunuzu oluşturabilirsiniz.
- Sıradaki geliştirme fikirleri: arama, karışık çal / tekrar, çalma listeleri,
  sürükle-bırak sıralama.
