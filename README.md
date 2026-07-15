# 🍋 Bahçe İkizi (Garden Digital Twin)

**Narenciye bahçeleri için uydu, drone ve IoT verisini tek bir dijital ikizde birleştiren akıllı tarım platformu**

[![Status](https://img.shields.io/badge/status-geli%C5%9Ftirme%20a%C5%9Famas%C4%B1nda-yellow)](#)
[![Faz](https://img.shields.io/badge/faz-1%20%2F%203-blue)](#-proje-yol-haritas%C4%B1-3-fazl%C4%B1-geli%C5%9Ftirme)
[![License](https://img.shields.io/badge/license-Proprietary-lightgrey)](#-lisans)
[![Stack](https://img.shields.io/badge/stack-FastAPI%20%7C%20React%20%7C%20PostGIS-informational)](#-teknoloji-yığını)

---

## 📖 İçindekiler

- [Proje Hakkında](#-proje-hakkında)
- [Neden Bu Proje?](#-neden-bu-proje)
- [Sistem Mimarisi](#-sistem-mimarisi)
- [Faz Bazlı Özellikler](#-faz-bazlı-özellikler)
  - [Faz 1 — Platform (Web + Mobil)](#faz-1--platform-web--mobil)
  - [Faz 2 — Drone Tabanlı Haritalama](#faz-2--drone-tabanlı-gelişmiş-haritalama-ve-verim-zekası)
  - [Faz 3 — IoT ve Akıllı Sulama](#faz-3--iot-sensörler-ve-akıllı-sulama)
- [Veri İşleme Pipeline'ı](#-veri-i̇şleme-pipelineı)
- [Dijital Sağlık Skoru — 8 Katmanlı Uydu Analizi](#-dijital-sağlık-skoru--8-katmanlı-uydu-analizi)
- [Harita Katmanları Evrimi](#-harita-katmanları-evrimi-3-faz-boyunca)
- [Teknoloji Yığını](#-teknoloji-yığını)
- [Proje Yapısı](#-proje-yapısı)
- [Kurulum](#-kurulum)
- [Yol Haritası](#-proje-yol-haritası-3-fazlı-geliştirme)
- [Vizyon](#-vizyon)
- [Katkıda Bulunma](#-katkıda-bulunma)
- [Lisans](#-lisans)
- [İletişim](#-i̇letişim)

---

## 🎯 Proje Hakkında

**Bahçe İkizi**, narenciye üreticileri için geliştirilen, bahçeyi uçtan uca dijitalleştiren bir **dijital ikiz (digital twin)** platformudur. Proje; uydu görüntüleme, drone tabanlı hassas tarım ve IoT sensör ağlarını **3 kademeli bir yol haritasıyla** tek bir haritada birleştirir:

> **Yer üstü** (uydu + drone) + **Yer altı** (toprak sensörleri) + **Aktif sistem** (akıllı sulama) = **Tek birleşik sağlık skoru**

Her faz bağımsız olarak geliştirilip test edilir ve bir önceki fazın üzerine **veri katmanı olarak** eklenir — böylece platform, düşük maliyetli bir web/mobil uygulamadan başlayıp tam otonom bir akıllı bahçe sistemine kadar kademeli olarak büyür. Mimari, farklı iklim koşullarına, bahçe büyüklüklerine ve narenciye türlerine uyarlanabilecek şekilde tasarlanmıştır.

---

## 🌱 Neden Bu Proje?

Geleneksel narenciye üretiminde çiftçiler şu sorunlarla karşılaşıyor:

| Problem | Bahçe İkizi Çözümü |
|---|---|
| Hastalık/su stresi geç fark ediliyor, bahçenin tamamı gezilemiyor | Uydu tabanlı NDVI/NDWI ısı haritaları ile kuşbakışı erken teşhis |
| Hasat miktarı ve geliri tahmin edilemiyor | Drone + YOLO ile ağaç başı meyve sayımı ve gelir projeksiyonu |
| İlaçlama tüm bahçeye yapılıyor, maliyet ve çevresel etki yüksek | Hastalık haritasına göre yalnızca etkilenen bölgeye yönelik akıllı ilaçlama planı |
| Sulama tahmine dayalı, su israfı var | IoT nem sensörleri ve akıllı vanalarla milimetrik, bölge bazlı sulama |
| Banka veya ihracat süreçlerinde tarlanın verimlilik kanıtı bulunmuyor | Tarla Pasaportu: geçmiş uydu verisine dayalı, PDF çıktılı verimlilik skoru |
| Çiftçi teknik desteğe anında ulaşamıyor | 7/24 AI Limon Doktoru chatbot ve gerektiğinde canlı ziraat mühendisi desteği |

---

## 🏗️ Sistem Mimarisi

### Yüksek Seviye Genel Bakış

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                             BAHÇE İKİZİ PLATFORM                             │
│                Narenciye Bahçeleri için Dijital İkiz Mimarisi                │
│                                                                              │
│                     ┌──────────────────────────────────┐                     │
│                     │        UNIFIED DASHBOARD         │                     │
│                     │     React Web  +  Mobile App     │                     │
│                     └────────────────┬─────────────────┘                     │
│                                      │                                       │
│             │                         │                         │            │
│             ▼                         ▼                         ▼            │
│  ┌────────────────────┐    ┌────────────────────┐    ┌────────────────────┐  │
│  │       TARLAM       │    │       PAZAR        │    │       TEKNİK       │  │
│  │   Bahçe Yönetimi   │    │   Piyasa Zekası    │    │     AI Destek      │  │
│  └────────────────────┘    └────────────────────┘    └────────────────────┘  │
│             │                         │                         │            │
│             └─────────────────────────┴─────────────────────────┘            │
│                                       ▼                                      │
│  ┌────────────────────────────────────────────────────────────────────────┐  │
│  │                 AI & VERİ KATMANI  —  FastAPI + Python                 │  │
│  │                                                                        │  │
│  │            ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐             │  │
│  │            │ NDVI│ │ NDWI│ │ EVI │ │ LAI │ │ LST │ │ SAR │             │  │
│  │            └─────┘ └─────┘ └─────┘ └─────┘ └─────┘ └─────┘             │  │
│  │                                                                        │  │
│  │                 ┌──────────┐ ┌──────────┐ ┌──────────┐                 │  │
│  │                 │Phenology │ │  YOLOv8  │ │Custom CNN│                 │  │
│  │                 └──────────┘ └──────────┘ └──────────┘                 │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                       ▼                                      │
│  ┌────────────────────────────────────────────────────────────────────────┐  │
│  │                       VERİ KAYNAKLARI & DONANIM                        │  │
│  │                                                                        │  │
│  │ ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐  │  │
│  │ │  Sentinel │ │    DJI    │ │    IoT    │ │    HKS    │ │  Weather  │  │  │
│  │ │   1 / 2   │ │   Drone   │ │ Sensörler │ │    API    │ │    API    │  │  │
│  │ └───────────┘ └───────────┘ └───────────┘ └───────────┘ └───────────┘  │  │
│  │                                                                        │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Detaylı Bileşen Şeması

```mermaid
graph TB
    subgraph Client["Istemci Katmani"]
        WEB["React + Mapbox<br/>Web Uygulamasi"]
        MOBILE["React Native / Flutter<br/>Mobil Uygulama"]
    end

    subgraph API["Uygulama Katmani"]
        GW["FastAPI / Node.js<br/>API Gateway"]
        AUTH["Supabase<br/>Auth ve Storage"]
        CACHE["Redis<br/>Cache"]
    end

    subgraph AI["Zeka Katmani"]
        CHATBOT["AI Limon Doktoru<br/>OpenAI API"]
        CNN["Hastalik Tespiti<br/>Custom CNN, ResNet-50"]
        YOLO["Meyve Sayimi<br/>YOLOv8, Detectron2"]
        ZONE["Verimlilik Zonlama<br/>scikit-learn, PyTorch"]
    end

    subgraph Data["Veri Kaynaklari"]
        S2["Sentinel-2<br/>Optik: NDVI, EVI, LAI"]
        S1["Sentinel-1<br/>SAR: Toprak Nem"]
        LANDSAT["Landsat-8, MODIS<br/>LST"]
        DRONE["Drone<br/>Multispektral ve RGB"]
        IOT["LoRaWAN Sensor Agi<br/>Nem, Sicaklik, EC"]
        MARKET["HKS ve Market Scraping<br/>Fiyat Verisi"]
    end

    subgraph Storage["Veri Katmani"]
        PG[("PostgreSQL + PostGIS<br/>Cografi Veri")]
        INFLUX[("InfluxDB<br/>Time-Series Sensor Verisi")]
    end

    subgraph Field["Sahada Donanim"]
        GATEWAY["LoRaWAN Gateway<br/>Raspberry Pi"]
        SENSOR["Toprak/Hava Sensorleri<br/>ESP32"]
        VALVE["Akilli Vanalar<br/>Solenoid + ESP32"]
        SOLAR["GES: Gunes Enerjisi"]
    end

    WEB --> GW
    MOBILE --> GW
    GW --> AUTH
    GW --> CACHE
    GW --> AI
    GW --> PG

    CHATBOT --> GW
    CNN --> DRONE
    YOLO --> DRONE
    ZONE --> S2

    S2 --> PG
    S1 --> PG
    LANDSAT --> PG
    DRONE --> PG
    MARKET --> PG

    SENSOR --> GATEWAY
    GATEWAY --> INFLUX
    INFLUX --> PG
    VALVE --> GATEWAY
    SOLAR --> VALVE
    SOLAR --> GATEWAY

    style Client fill:#e8f5e9
    style API fill:#e3f2fd
    style AI fill:#fff3e0
    style Data fill:#fce4ec
    style Storage fill:#f3e5f5
    style Field fill:#efebe9
```

---

## 📦 Faz Bazlı Özellikler

Proje **3 bağımsız fazda** geliştirilir; her faz kendi içinde tamamlanır, test edilir ve bir önceki fazla birleştirilir.

```mermaid
gantt
    title Bahce Ikizi - Gelistirme Fazlari
    dateFormat  X
    axisFormat  %s
    section Faz 1
    Platform (Web + Mobil)         :active, f1, 0, 3
    section Faz 2
    Drone ve Verim Zekasi          :f2, after f1, 3
    section Faz 3
    IoT ve Akilli Sulama           :f3, after f2, 3
```

### Faz 1 — Platform (Web + Mobil)

**Hedef:** Uydu verisiyle çalışan, hemen değer üreten bir MVP.

<details>
<summary><b>🗺️ Tarlam — Dijital Operasyon Merkezi</b></summary>

- Harita üzerinde parsel sınırı (poligon) çizme, ağaç sayısı/tür/yaş girişi
- **8 katmanlı uydu sağlık analizi:** NDVI, NDWI, EVI, LAI, LST, SAR toprak nemi, Phenology (mevsimsel takip), 10 yıllık zaman serisi trend analizi
- **Tarla Pasaportu:** Geçmiş verimlilik skoru (100 üzerinden), banka/kira için PDF kanıt raporu
- İşletme & finans takibi (işçilik, gider, kâr/zarar)
- **Üretim Kayıt Defteri:** Barkod okuma ile ilaç/dozaj kaydı, AB ihracat uyumluluk raporu
- Offline saha defteri (fotoğraf/ses notu, internet gelince senkron)
</details>

<details>
<summary><b>📈 Pazar ve Haber Zekası</b></summary>

- **Limon Borsası:** HKS (Hal Kayıt Sistemi) + market fiyat scraping (Migros, Bim, A101...) karşılaştırması
- Küresel sektör bülteni (İspanya, Arjantin, G. Afrika rekolte/fiyat verisi)
- Yerel sektör bülteni (fuarlar, devlet destekleri)
- AI Haber Asistanı: haberin çiftçiye etkisini 2 cümlede özetler
</details>

<details>
<summary><b>🩺 Teknik Destek ve Uzmanlık</b></summary>

- **AI Limon Doktoru:** Sola (OneSoil) tarzı chatbot arayüzü, fotoğrafla hastalık teşhisi
  - Tespit edilen hastalıklar: Trips, Limon Uyuzu, Kırmızı Örümcek, Kök Çürüklüğü, Apaç (Kanser)
- Uzmanına Sor: AI'nın çözemediği durumlarda ziraat mühendisi randevusu
- Hastalık veritabanı (Faz 2 AI model eğitimi için etiketlenmiş veri)
</details>

<details>
<summary><b>⚖️ Karar Destek ve Verimlilik</b></summary>

- Bahçeye özel budama/sulama/hasat takvimi + push bildirimleri
- Hassas dozaj hesaplayıcı (ağaç sayısı × tür × ilaç markası)
- PHI (İlaç Etkinlik Süresi) geri sayımı + AB MRL uyarısı
- Karşılaştırmalı meteoroloji, don/afet risk uyarısı, 7 günlük tahmin
</details>

<details>
<summary><b>👥 Topluluk, Rehber ve Devlet Entegrasyonu</b></summary>

- Agri-Akademi: 1 dakikalık sertifikalı eğitim videoları
- Servis/ekipman rehberi (traktör ustası, gübre bayisi, lojistik)
- Çiftçi Meydanı: moderasyonlu forum
- **E-Ziraat:** T.C. Tarım Bakanlığı / ÇKS / TARSİM entegrasyonu, destek başvuru takibi
- Hal entegrasyonu + soğuk hava deposu takibi
</details>

### Faz 2 — Drone Tabanlı Gelişmiş Haritalama ve Verim Zekası

**Hedef:** Tarlayı "hasat planı çıkaran bir makineye" dönüştürmek.

| Modül | Teknoloji | Çıktı |
|---|---|---|
| **Meyve Sayımı** | YOLOv8 / Faster R-CNN, 50-80m irtifa, orthomosaic | Ağaç başı meyve sayısı, toplam rekolte tahmini (yüksek doğruluk) |
| **Boyut & Olgunluk Analizi** | RGB→HSV piksel/renk analizi | İhracatlık / iç piyasa / sanayi kalite dağılımı |
| **Gelir Projeksiyonu** | HKS + market fiyat entegrasyonu | İyimser / gerçekçi / kötümser senaryolu gelir dashboard'u |
| **Ağaç Başı Numaralandırma** | Computer vision + GPS RTK | Benzersiz ID, cm hassasiyetinde konum |
| **DSM/DTM Analizi** | Fotogrametri | Eğim, su akış yönü, erozyon riski, ağaç hacmi |
| **Hastalık Tespiti** | Custom CNN (ResNet-50 backbone) | Ağaç başı hastalık haritası + yayılım yönü/hotspot analizi |
| **Otomatik İlaçlama Planı** | Hastalık haritası → dozaj motoru | Sadece hasta bölgeye ilaçlama ile ilaç, zaman ve su tasarrufu |
| **Karbon (MRV)** | DSM hacim modeli + girdi bazlı emisyon hesabı | Verra VCS / Gold Standard uyumlu PDF rapor, karbon kredisi potansiyeli |

### Faz 3 — IoT, Sensörler ve Akıllı Sulama

**Hedef:** Tamamen otonom, güneş enerjili akıllı bahçe.

```mermaid
flowchart LR
    A["Toprak/Hava Sensorleri<br/>15-30-60cm derinlik"] -->|LoRaWAN| B["Gateway<br/>Raspberry Pi"]
    B -->|MQTT| C["InfluxDB<br/>Time-Series"]
    C --> D{"Nem esigi kontrolu"}
    D -->|Esik altinda| E["Akilli Vana<br/>Solenoid + ESP32"]
    D -->|Yeterli| F["Bekleme"]
    E --> G["Sondaj Pompa<br/>Kontrol Unitesi"]
    H["GES Gunes Paneli"] -->|Enerji| B
    H -->|Enerji| E
    H -->|Enerji| G
```

- **Sensör Ağı:** 3 derinlikte (15/30/60cm) nem, sıcaklık, EC; hava istasyonu
- **LoRaWAN İletişim:** İnternetsiz bahçelerde düşük enerjili, uzun menzilli veri akışı
- **Akıllı Vana:** Bölge bazlı otomatik sulama, manuel override
- **Sondaj/Pompa Kontrolü:** Kuru çalışma, aşırı basınç, voltaj koruması
- **GES (Güneş Enerjisi):** Şebeke bağımsız, batarya yedekli sistem
- **Veri Monetizasyonu:** Anonim veri satışı (meteoroloji, gübre, ilaç, sigorta şirketlerine), kooperatif raporları, karbon kredisi danışmanlığı
- **SaaS/Platform Genişlemesi:** Temel/Profesyonel/Kurumsal paketler, beyaz etiket kooperatif çözümü, API ekosistemi

---

## 🔄 Veri İşleme Pipeline'ı

```mermaid
flowchart TD
    subgraph Kaynak["1. Veri Kaynagi"]
        SH["Sentinel Hub API /<br/>Google Earth Engine"]
    end

    subgraph Filtre["Bulut Filtreleme"]
        CF["Fmask / S2Cloudless"]
    end

    subgraph Islem["2. Isleme Motoru - Python"]
        GDAL["GDAL + Rasterio + NumPy"]
        NDVI_CALC["Indeks Hesabi<br/>NDVI = NIR-Red / NIR+Red"]
        ZONE_CALC["Zonlama<br/>10 yillik istatistiksel ortalama"]
    end

    subgraph Dogrulama["3. Dogrulama - Data Fusion"]
        PG[("PostgreSQL + PostGIS")]
        INFLUX[("InfluxDB - Sensor")]
        FUSION{"Uydu vs sensor<br/>celiskisi?"}
    end

    subgraph AILayer["4. Akilli Analiz"]
        ML["scikit-learn / TensorFlow<br/>Verimlilik Zonlama"]
    end

    subgraph Hibrit["5. Hibrit Dogrulama"]
        S1R["Sentinel-1 SAR<br/>bulutlu gunlerde devreye girer"]
        IOT_BACKUP["IoT sensor<br/>yerel yedek veri"]
        INTERP["AI Interpolasyon"]
    end

    subgraph Arayuz["6. Arayuz"]
        MAP["Mapbox / Leaflet<br/>Katman Secici"]
    end

    SH --> CF --> GDAL --> NDVI_CALC --> ZONE_CALC --> PG
    INFLUX --> FUSION
    PG --> FUSION
    FUSION -->|Sensor verisi oncelikli| PG
    PG --> ML --> MAP
    CF -.->|Bulutlu gun| S1R --> INTERP
    IOT_BACKUP --> INTERP
    INTERP --> PG
```

**Kritik prensip:** Uydu verisi tek başına *tahmindir*. Bunu *kesin bilgiye* dönüştürmek için IoT sensör verisiyle çapraz doğrulama yapılır — çelişki durumunda sensör verisi esas alınır (çünkü toprağın 30cm altını doğrudan ölçer).

---

## 🛰️ Dijital Sağlık Skoru — 8 Katmanlı Uydu Analizi

| # | İndeks | Ölçtüğü | Formül / Yöntem | Kaynak |
|---|---|---|---|---|
| 1.1 | **NDVI** | Bitki sağlığı/yoğunluğu | `(NIR - Red) / (NIR + Red)` | Sentinel-2 B8/B4 |
| 1.2 | **NDWI** | Su içeriği | `(NIR - SWIR) / (NIR + SWIR)` | Sentinel-2 B8/B11 |
| 1.3 | **EVI** | Gelişmiş bitki sağlığı (sık dikim) | `2.5 × (NIR-Red)/(NIR+6×Red-7.5×Blue+1)` | Sentinel-2 |
| 1.4 | **LAI** | Yaprak yoğunluğu / fotosentez kapasitesi | PROSAIL modeli / NDVI-LAI regresyonu | Sentinel-2 B3/B4/B5 |
| 1.5 | **LST** | Toprak yüzey sıcaklığı | Split-Window algoritması | Landsat-8 TIRS / MODIS |
| 1.6 | **SAR Toprak Nemi** | Bulut altı nem seviyesi | Water Cloud Model | Sentinel-1 VV/VH |
| 1.7 | **Phenology** | Çiçeklenme/hasat dönemi tespiti | Savitzky-Golay filtreleme (SOS/EOS) | NDVI zaman serisi |
| 1.8 | **Zaman Serisi Trend** | Aylık/yıllık gelişim karşılaştırması | 10 yıllık istatistiksel zonlama | Tüm indeksler |

---

## 🗺️ Harita Katmanları Evrimi (3 Faz Boyunca)

```mermaid
flowchart TB
    subgraph F1["Faz 1 Sonrasi - Temel Uydu Haritasi"]
        L1["NDVI, NDWI, EVI, LAI, LST, SAR<br/>Phenology, Parsel Poligonu"]
    end
    subgraph F2["Faz 2 Sonrasi - Drone Detay Haritasi"]
        L2["Faz 1 verileri<br/>Agac Basi Numara ve Saglik<br/>Hastalik Haritasi, DSM/Egim<br/>Karbon, Verim Tahmini, Meyve Sayimi"]
    end
    subgraph F3["Faz 3 Sonrasi - Birlesik Dijital Ikiz"]
        L3["Faz 1 ve 2 verileri<br/>Yer Alti: Nem, Sicaklik, pH, EC<br/>Aktif Sistem: Vana Durumu, GES<br/>Tek Birlesik Saglik Skoru"]
    end
    F1 -->|Drone verisi eklenir| F2
    F2 -->|IoT verisi eklenir| F3

    style F1 fill:#e3f2fd
    style F2 fill:#e8f5e9
    style F3 fill:#fff3e0
```

---

## 🧰 Teknoloji Yığını

<table>
<tr><th>Katman</th><th>Faz</th><th>Teknolojiler</th></tr>
<tr><td><b>Backend</b></td><td>1</td><td>FastAPI (Python) / Node.js</td></tr>
<tr><td><b>Veritabanı</b></td><td>1</td><td>PostgreSQL + PostGIS (coğrafi veri)</td></tr>
<tr><td><b>Cache</b></td><td>1</td><td>Redis</td></tr>
<tr><td><b>Auth & Storage</b></td><td>1</td><td>Supabase</td></tr>
<tr><td><b>Frontend Web</b></td><td>1</td><td>React + Mapbox</td></tr>
<tr><td><b>Frontend Mobil</b></td><td>1</td><td>React Native / Flutter</td></tr>
<tr><td><b>AI (Chatbot/Özet)</b></td><td>1</td><td>OpenAI API</td></tr>
<tr><td><b>Scraping</b></td><td>1</td><td>Scrapy</td></tr>
<tr><td><b>Uydu Verisi</b></td><td>1</td><td>Sentinel-2 API, Sentinel-1 SAR API, MODIS/Landsat</td></tr>
<tr><td><b>Drone SDK</b></td><td>2</td><td>DJI SDK / Pix4D</td></tr>
<tr><td><b>Görüntü İşleme</b></td><td>2</td><td>OpenCV, TensorFlow</td></tr>
<tr><td><b>Haritalama</b></td><td>2</td><td>OpenDroneMap / QGIS</td></tr>
<tr><td><b>Hastalık Tespiti</b></td><td>2</td><td>Custom CNN (ResNet-50 backbone, transfer learning)</td></tr>
<tr><td><b>Nesne Algılama</b></td><td>2</td><td>YOLOv8 / Detectron2</td></tr>
<tr><td><b>Sensör İletişimi</b></td><td>3</td><td>LoRaWAN (SX1276/SX1262)</td></tr>
<tr><td><b>Gateway</b></td><td>3</td><td>Raspberry Pi + LoRaWAN HAT / TTN Gateway</td></tr>
<tr><td><b>Real-time Data</b></td><td>3</td><td>MQTT + InfluxDB (time-series)</td></tr>
<tr><td><b>Edge Computing</b></td><td>3</td><td>ESP32</td></tr>
<tr><td><b>Enerji</b></td><td>3</td><td>Solar panel + MPPT + LiFePO4 batarya</td></tr>
<tr><td><b>Hava Durumu API</b></td><td>Tümü</td><td>OpenWeatherMap / Meteoblue</td></tr>
<tr><td><b>Dış Entegrasyonlar</b></td><td>Tümü</td><td>HKS API, E-Devlet/Tarım Bakanlığı, Ziraat Bankası, TARSİM, Stripe/PayTR</td></tr>
</table>

---

## 📁 Proje Yapısı

> Aşağıdaki yapı, önerilen monorepo organizasyonunu gösterir — repodaki gerçek klasörlere göre güncelleyin.

```
bahce-ikizi/
├── apps/
│   ├── web/                 # React + Mapbox web uygulaması
│   └── mobile/               # React Native / Flutter mobil app
├── services/
│   ├── api-gateway/           # FastAPI ana servis
│   ├── satellite-pipeline/    # Sentinel-1/2 veri çekme & indeks hesaplama
│   ├── drone-processing/      # Orthomosaic, CNN, YOLO servisleri (Faz 2)
│   ├── iot-ingestion/         # MQTT/LoRaWAN veri toplama (Faz 3)
│   └── chatbot/               # AI Limon Doktoru servisi
├── infra/
│   ├── postgis/                # Veritabanı şema & migration
│   ├── influxdb/               # Time-series konfigürasyonu
│   └── docker-compose.yml
├── firmware/                   # ESP32 / sensör kodları (Faz 3)
├── docs/
│   └── bahce-ikizi-dokumantasyon.pdf
└── README.md
```

---

## ⚙️ Kurulum

> Aşağıdaki adımlar örnek bir yerel geliştirme ortamı kurulumunu gösterir.

```bash
# Depoyu klonlayın
git clone https://github.com/<kullanici-adi>/bahce-ikizi.git
cd bahce-ikizi

# Backend bağımlılıkları
cd services/api-gateway
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt

# Ortam değişkenlerini ayarlayın
cp .env.example .env
# SENTINEL_HUB_CLIENT_ID, SENTINEL_HUB_CLIENT_SECRET,
# SUPABASE_URL, SUPABASE_KEY, OPENAI_API_KEY vb. doldurun

# Veritabanını başlatın (PostGIS uzantılı PostgreSQL)
docker-compose up -d postgis redis

# Backend'i çalıştırın
uvicorn main:app --reload

# Frontend
cd ../../apps/web
npm install
npm run dev
```

---

## 🛣️ Proje Yol Haritası (3 Fazlı Geliştirme)

- [x] **Faz 1 — Platform:** Tarlam modülü, 8 katmanlı uydu analizi, Tarla Pasaportu, AI Limon Doktoru, pazar zekası, E-Ziraat entegrasyonu
- [ ] **Faz 2 — Drone & Verim Zekası:** Meyve sayımı (YOLO), hastalık haritalama (CNN), otomatik ilaçlama planı, MRV karbon raporu
- [ ] **Faz 3 — IoT & Akıllı Sulama:** LoRaWAN sensör ağı, akıllı vana kontrolü, GES entegrasyonu, veri monetizasyonu, SaaS/kooperatif modeli

---

## 🔭 Vizyon

Bahçe İkizi, tek bir bahçeden başlayıp ölçeklenebilir bir global akıllı tarım altyapısı olmayı hedefler:

- Dünya genelindeki narenciye üreticilerine ulaşan, yerelden bağımsız çalışabilen bir **SaaS platformu** olmak
- Uydu, drone ve IoT verisini birleştiren **açık, veriye dayalı hassas tarım standardını** belirlemek
- Karbon kredisi ve sürdürülebilirlik raporlamasında (MRV) üreticiler için **uluslararası ölçekte erişilebilir** bir çözüm sunmak
- Kooperatifler, ihracatçılar ve finans kuruluşları için **güvenilir tarımsal veri altyapısı** haline gelmek
- Su, ilaç ve enerji kullanımını optimize ederek **sürdürülebilir narenciye üretimine** küresel ölçekte katkı sağlamak

---

## 🤝 Katkıda Bulunma

Bu proje şu anda aktif geliştirme aşamasındadır. Katkı sağlamak isterseniz:

1. Depoyu fork'layın
2. Özellik dalınızı oluşturun (`git checkout -b ozellik/yeni-ozellik`)
3. Değişikliklerinizi commit'leyin (`git commit -m 'Yeni özellik eklendi'`)
4. Dalınızı push'layın (`git push origin ozellik/yeni-ozellik`)
5. Pull Request açın

---

## 📄 Lisans

Bu proje için lisans bilgisi eklenecektir. *(Örn: MIT, Apache 2.0 veya Proprietary — tercihinize göre güncelleyin.)*

---

## 📬 İletişim

Sorularınız veya iş birliği teklifleriniz için proje sahibiyle iletişime geçebilirsiniz.

---

<p align="center">
  <sub>Proje: Bahçe İkizi (Garden Digital Twin) · Akıllı Narenciye Tarımı Platformu</sub>
</p>
