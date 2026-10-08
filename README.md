# BSM 461 — Gerçek Zamanlı Satış ve Stok Takibi

> **Durum:** Hazırlık aşaması. Başlangıç kiti henüz temin edilmedi; veri hattı kurulmuş veya test edilmiş değildir.

## Proje bilgileri

| Alan | Bilgi |
| --- | --- |
| Ders | BSM 461 — Büyük Veriye Giriş |
| Dönem | 2026–2027 Güz |
| Şablon | Şablon 2 — Gerçek zamanlı satış ve stok takibi |
| Öğretim / grup | 1. öğretim / A grubu |
| Grup kodu | `B231210038` (en küçük öğrenci numarası) |
| Üyeler | **Tamamlanacak:** üç üyenin adı ve öğrenci numarası |
| Son teslim | **11 Aralık 2026 Cuma, 23.59** |
| Teslim | Rapor ve sunum PDF: SABIS; kod ve kanıtlar: public GitHub |

Bu depo dersin *Proje-Odevi.pdf* yönergesindeki **Z0–Z12** bileşenleri için hazırlanmıştır. Tüm rapor sonuçları grup koduna özgü veri ve **gerçek ölçümlerle** desteklenecektir. Başka grupların ölçümleri kullanılmayacaktır.

## Hedef

Mağazalardan gelen **satış, iade, stok girişi** olaylarını hem akış olarak (Kafka + Spark Structured Streaming) hem de geçmiş veri olarak (S3 uyumlu nesne deposu + Spark + Parquet/Delta) işlemek; sonuçları bir servis deposunda tutup panoda göstermek.

### Veri kaynakları (yönergede belirtilenler)

- Olaylar: `olay_zamani`, `siparis_id`, `magaza_id`, `urun_id`, `adet`, `birim_fiyat`, `tur`.
- Başlangıç stoğu: `s3://ham/satis_baslangic_stok.csv`.
- Boyut tabloları: `urunler.csv` (kategori, marka, maliyet) ve `magazalar.csv` (şehir, metrekare).
- Gruba özel veri ve senaryo: hocanın kitindeki `araclar/uretici.py` ve `araclar/senaryo.py` komutları.

### Başlangıç analiz soruları

**Akış (en az iki soru):**
1. Hangi ürünlerin mevcut stoğu senaryo kartından türetilen kritik eşiğin altına düştü?
2. Her mağazanın son bir saatlik net cirosu nedir?

**Toplu (en az iki soru):**
1. Seçilen tarih aralığında en çok satılan 10 ürün hangileri?
2. Hangi mağazaların iade oranı daha yüksek?

> Zaman pencereleri, stok eşiği, hesaplama kuralları ve gecikme hedefleri **senaryo kartı görüldüğünde** netleştirilecek. Tekrar eden olayların stok/ciro hesaplarını iki kez etkilememesi ayrıca test edilecek.

## Taslak mimari

```text
                             +-----------------------+
                             | Veri üretici (Python) |
                             +----------+------------+
                                        |
                  +---------------------+----------------------+
                  |                                            |
                  v                                            v
           Kafka / satislar                        SeaweedFS / S3 ham veri
                  |                                            |
                  v                                            v
        Spark Structured Streaming                      Spark toplu iş
          (pencere, watermark,                        (temizleme, boyut join,
           tekrar ayıklama)                            aggregation, explain)
                  |                                            |
                  v                                            v
       Akış sonuçları / servis                        Parquet + Delta tabloları
                  |                                            |
                  +---------------------+----------------------+
                                        |
                                        v
                              Servis deposu + pano
```

**Not:** Bu bir taslaktır. Z0 için bileşen sürümleri, bağlantıların kopya sayıları, sıralama/teslim garantileri ve ağ bölünmesi davranışları daha sonra ölçülerek belgelenecek.

## Başlangıç kiti geldiğinde

Hocanın başlangıç kiti **henüz bu repoda yoktur**. Kit edinildiğinde `kit/README.md` ile bütünleşen asıl dosyalar (özellikle `docker-compose.yml`, `araclar/`, `is/`) eklenecek. Komutları **`docker-compose.yml` dosyasının bulunduğu dizinde** çalıştırın.

Yönergede verilen temel komutlar (kit olmadan çalışmaz):

```bash
docker compose up -d
./dogrula.sh --tam
docker compose exec araclar python araclar/senaryo.py --grup B231210038 --sablon satis
```

Windows PowerShell'de `./dogrula.sh --tam` yerine yönergede belirtilen doğrulama:

```powershell
docker compose exec araclar python araclar/dogrula.py
docker compose exec spark spark-submit is/dogrula_spark.py
```

**Gereksinim:** Docker Desktop; bilgisayarda en az 8 GB RAM, Docker'a en az 5 GB bellek (Elasticsearch/Kibana ile 7 GB). Önerilen ek araçlar: Git ve VS Code.

## Dosya düzeni

- `is/` — Spark toplu ve akış işleri (kit gelince gerçek kod yerleştirilecek).
- `analiz/` — iki toplu sorunun sonuçlarını gösteren Python/R grafik betikleri.
- `kanit/` — senaryo, ortam doğrulaması, explain çıktısı, Spark UI görüntüleri, ölçümler, kırılma deneyi.
- `dokuman/` — aşama planı, kararlar ve deney kontrol listesi.
- `.gitignore` — büyük veri dosyaları, kişisel ortam ayarları ve geçici çıktılar.

## Ölçüm ve teslim kuralları

- Her performans iddiası ölçüm, birim ve deney koşuluyla desteklenir; sonuçlar `kanit/olcumler.csv` dosyasına işlenir.
- **Z11:** En az üç kırılma noktası belirlenecek, en az biri gerçekten denenip `kanit/kirilma-deneyi.*` içinde belgelenecek.
- **Z8:** Geç veri oranı ve yeniden başlatma / teslim garantisi deneyleri yapılacak.
- **Z7:** Delta sürümleri, eski sürümü okuma ve yarıda kalan yazma deneyi yapılacak.
- README ile sıfırdan çalıştırma teslimden önce grup üyesi tarafından doğrulanacak.
- Rapor ekler hariç en çok **25 sayfa**; sunum ilk slaytı tamamlanmış mimari.
- Yapay zekâ kullanım beyanı ve üç üyenin katkı beyanı zorunlu.
- Teslimden hemen önce `git tag teslim`, `git push origin teslim` ve `git rev-parse teslim` uygulanarak commit kimliği rapor kapağına yazılacak. **Bu etiketi şimdi oluşturmayın.**

## Sonraki adım

1. Kalan iki grup üyesini ve GitHub kullanıcı adlarını netleştirmek.
2. Hocanın `kit/` başlangıç dosyalarını temin etmek.
3. Ortamı çalıştırıp doğrulama kanıtı oluşturmak.
4. Gruba özel senaryo kartını üretip `kanit/senaryo.txt` içine kaydetmek.
5. Veri sözlüğü, ilk dört soru, ölçek hesabı ve mimari taslağını tamamlamak.

**Uyarı:** Bu depoda henüz çalışır Kafka/Spark sistemi veya ölçülmüş performans sonucu bulunmuyor.
