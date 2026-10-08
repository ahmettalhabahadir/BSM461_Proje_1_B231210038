# BSM461 — Çalışma Planı (Şablon 2)

**Durum:** Başlangıç kiti bekleniyor; bu belge görev planıdır, test sonucu değildir.

## 1. Grup ve kurulum
- [x] Şablon 2 seçildi.
- [x] Grup kodu: B231210038 (1. öğretim, A grubu).
- [x] Public GitHub deposu oluşturuldu.
- [ ] Üç üyenin kimlikleri ve GitHub erişimleri tamamlandı.
- [ ] Hocanın kit/ başlangıç dosyaları temin edildi.
- [ ] Docker Desktop ile kurulum yapılıp doğrulama görüntüsü kaydedildi.

## 2. Veri ve ölçek (Z1, Z10)
- [ ] `araclar/senaryo.py --grup B231210038 --sablon satis` çıktısı `kanit/senaryo.txt` dosyasına kaydedildi.
- [ ] Olay alanları için tip, açıklama ve örnek içeren veri sözlüğü hazırlandı.
- [ ] Çarpık anahtar, geç olay, tekrar ve kirli kayıt için gerçek örnekler bulundu.
- [ ] Ortalama/tepe olay hızı ve olay boyutu ölçülerek saklama hacmi hesaplandı.
- [ ] KVKK notu yazıldı.

## 3. Depolama ve toplu iş (Z2–Z5, Z7)
- [ ] Ham JSON üretildi; Parquet'e çevrildi.
- [ ] JSON/Parquet ve snappy/zstd/gzip karşılaştırmaları ölçüldü.
- [ ] Nesne deposunda bölümleme ve küçük dosya problemi incelendi.
- [ ] Sekiz bölümde dağılım, sıcak anahtar ve tekrar olay etkisi ölçüldü.
- [ ] Boyut tablosuyla join, en az iki toplu analiz sorgusu tamamlandı.
- [ ] Spark explain(), UI kanıtı ve üçer tekrar ile önce/sonra optimizasyon ölçümleri kaydedildi.
- [ ] Delta Lake iki sürüm, zaman yolculuğu ve kesilen yazma deneyi yapıldı.

## 4. Akış, servis ve pano (Z6, Z8, Z9)
- [ ] Kafka bölüm sayısı, anahtar, sıra garantisi hesaplarla gerekçelendirildi.
- [ ] İki akış sorusu Spark Structured Streaming ile yanıtlandı.
- [ ] Pencere, watermark ve geç olay deneyi tamamlandı.
- [ ] Checkpoint, idempotent yazma ve yeniden başlatma test edildi.
- [ ] Servis deposu erişim tablosuna göre seçildi ve dolduruldu.
- [ ] Satış/stok panosu ve ekran görüntüleri hazırlandı.

## 5. Mimari, kırılma ve teslim (Z0, Z11, Z12)
- [ ] Son mimari çizildi; sürümler ve her okun garanti tablosu dolduruldu.
- [ ] En az üç kırılma noktası açıklandı.
- [ ] En az bir kırılma deneyinin gerçek çıktı/görüntüsü kaydedildi.
- [ ] En az iki grafik ve açıklamaları rapora aktarıldı.
- [ ] 25 sayfa sınırına uygun rapor ve sunum PDF'leri hazırlandı.
- [ ] YZ kullanım beyanı ve imzalı katkı beyanı hazırlandı.
- [ ] README adımlarından sıfırdan kurulum test edildi.
- [ ] Son commit için `teslim` etiketi oluşturuldu ve rapora commit SHA yazıldı.
- [ ] 11 Aralık 2026, 23.59 öncesinde SABIS + GitHub teslimi tamamlandı.

## Teknik karar notları

Kararların hiçbiri tahmini ölçümle savunulmayacak. Senaryo kartı gelmeden stok eşiği, pencere/watermark süresi ve veri hızları kesinleştirilmeyecek.
