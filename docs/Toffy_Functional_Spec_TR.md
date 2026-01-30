# TOFFY MANUFACTURING | Fonksiyonel Spesifikasyon

**Sayfa 1**

# TOFFY MANUFACTURING
**Gizli - Sadece Müşteri Dahili Kullanımı İçindir**

## Fonksiyonel Spesifikasyon
**Stok Özeti Raporu (BTP ABAP)**
(Stock Snapshot Report)

*   **Belge ID:** TFY-MM-FS-SS-001
*   **Versiyon:** 0.1 (Kapalı Beta)
*   **Tarih:** 2026-01-17
*   **Yazar:** MM Danışmanı (Toffy Programı)
*   **Modül:** MM (Envanter Yönetimi / Depo Görünürlüğü)
*   **Hedef Platform:** SAP BTP ABAP Ortamı (Trial)

---

**Sayfa 2**

## İçindekiler

1.  Amaç ve Arka Plan
2.  İş Senaryosu (Toffy Manufacturing)
3.  Kapsam ve Hedefler
4.  Roller ve Erişim
5.  Süreç Genel Bakışı
6.  Fonksiyonel Gereksinimler
7.  Veri Modeli (ZLI Tabloları)
8.  UI Spesifikasyonu (Fiori Uygulaması)
9.  Doğrulamalar ve Hata Yönetimi
10. Fonksiyonel Olmayan Gereksinimler
11. Kabul Kriterleri (Acceptance Criteria)
12. Açık Noktalar ve Varsayımlar
13. Ek A: Alan Kataloğu

---

**Sayfa 3**

## 1. Amaç ve Arka Plan
Bu belge, SAP BTP ABAP üzerinde geliştirilecek olan Stok Özeti Raporu (Stock Snapshot Report) için gereksinimleri tanımlar.
Rapor, Toffy Manufacturing kullanıcılarının envanter durumunu (miktarlar ve değer) incelemesini ve bir öğrenme simülasyonuna uygun, hızlı ve yönlendirilmiş bir akışla düşük stok risklerini belirlemesini sağlar.
Çözüm, kasıtlı olarak bir on-premise S/4HANA sistemine erişim olmadan çalışacak şekilde tasarlanmıştır. Tüm veriler, BTP ABAP ortamında oluşturulan özel **ZLI tablolarında** saklanacaktır.

## 2. İş Senaryosu (Toffy Manufacturing)
Toffy; çikolata, bisküvi ve şekerleme üreten bir Hızlı Tüketim Malları (FMCG) üreticisidir. Pilot proje, yüksek hacmin küçük veri hatalarını gerçek olaylara dönüştürdüğü **Üretim Yeri 1000**'e odaklanmaktadır.

İş birimi, günlük operasyonlar için bir "Stok Özeti" ekranı talep etmektedir. Bu ekran, bir başlık özeti (snapshot üst verisi) ve basit bir grafikle birlikte malzeme listesini göstermelidir.

**Operasyonel hedef:** Yeniden tedarik kontrolleri sırasında karar verme süresini kısaltmak ve kritik ambalaj malzemelerinde stok tükenmesini (stock-out) önlemektir.

*Not: Bazı pilot paydaşları gelecekteki yaygınlaştırma için Üretim Yeri 1100'den de bahsetmektedir; rapor buna uyumlu olmalıdır.*

## 3. Kapsam ve Hedefler
**Kapsam Dahili:**
*   Snapshot sonuçlarını saklamak için özel veri modeli (başlık + kalem) oluşturmak.
*   Snapshot başlığını, kalem listesini ve bir grafiği görüntülemek için bir Fiori uygulaması sağlamak.
*   Verileri ZLI tablolarına hesaplayan bir "Snapshot Oluştur" (Generate Snapshot) aksiyonunu etkinleştirmek.
*   Üretim Yeri ve Snapshot Tarihine göre filtrelemeyi desteklemek.

**Kapsam Harici (Kapalı Beta):**
*   Gerçek S/4HANA envanter tablolarına (örn. MARD) entegrasyon.
*   Depo Yönetimi (WM) / EWM entegrasyonu.
*   Karmaşık değerleme ve para birimi dönüştürme.

## 4. Roller ve Erişim

| Rol | Ana Aktiviteler | Erişim |
| :--- | :--- | :--- |
| **Envanter Analisti** | Snapshot'ları görüntüleme, kalemleri filtreleme, listeyi indirme. | Okuma (Read) |
| **MM Ana Kullanıcısı** | Snapshot oluşturma, istisnaları inceleme, yorum ekleme. | Okuma + Oluşturma |
| **Geliştirici (Simülasyon)** | Veri modeli oluşturma, oluşturma mantığını uygulama, birim testleri. | Tam (BTP dev) |

---

**Sayfa 4**

## 5. Süreç Genel Bakışı
**Üst seviye akış:**
1.  Kullanıcı Stok Özeti (Stock Snapshot) uygulamasını açar.
2.  Kullanıcı Üretim Yeri ve Snapshot Tarihini seçer (varsayılan: bugün).
3.  Kullanıcı "Snapshot Oluştur" (Generate Snapshot) işlemini çalıştırır (başlık + kalemler oluşur).
4.  Kullanıcı riske göre sıralanmış kalemleri inceler (düşük stok en başta).
5.  Kullanıcı en çok eksik olan 5 kalemi ve trendi grafikten kontrol eder.

## 6. Fonksiyonel Gereksinimler

| ID | İsim | Açıklama |
| :--- | :--- | :--- |
| **FR-001** | Snapshot Başlığı | Sistem, Üretim Yeri + Tarih başına toplamları ve üst verileri içeren bir başlık kaydı saklar. |
| **FR-002** | Snapshot Kalemleri | Sistem, kapsamdaki her malzeme için miktar, birim ve değer içeren kalem kayıtlarını saklar. |
| **FR-003** | Risk İşaretleme | Miktar < Yeniden Sipariş Eşiği olduğunda sistem kalemleri **LOW** (Düşük) olarak işaretler. |
| **FR-004** | Grafik | Sistem, "En Düşük 5 Kalem" grafiğini ve miktar trendini görüntüler. |
| **FR-005** | Dışa Aktarma | Kullanıcı kalem listesini CSV'ye aktarabilir. |
| **FR-006** | Yorumlar | MM Ana Kullanıcısı snapshot başlığına bir kısa yorum ekleyebilir. |

## 7. Veri Modeli (ZLI Tabloları)
Çözüm üç özel tablo kullanır. İsimlendirme örnektir ve paket kurallarına uyacak şekilde ayarlanabilir.

### 7.1 Tablo: ZTF_STOCK_HDR (Snapshot Başlığı)
**Birincil Anahtar:** `(snapshot_uuid)`

| Alan | Anahtar | ABAP Tipi | Örnek | Notlar |
| :--- | :--- | :--- | :--- | :--- |
| **SNAPSHOT_UUID** | X | RAW16 | 16-byte UUID | Oluşturulurken üretilir. |
| **PLANT** | | CHAR4 | 1000 | Toffy fabrikası. |
| **SNAPSHOT_DATE** | | DATS | 2026-01-17 | Snapshot tarihi. |
| **TOTAL_ITEMS** | | INT4 | 120 | Kalem satır sayısı. |
| **TOTAL_VALUE** | | DEC(15,2) | 125000.00 | Envanter değeri (veya para birimi). |
| **CURRENCY** | | CUKY | EUR | Para birimi kodu. |

---

**Sayfa 5**

| Alan | Anahtar | ABAP Tipi | Örnek | Notlar |
| :--- | :--- | :--- | :--- | :--- |
| **COMMENT** | | STRING(200) | "Ambalaj riski" | Opsiyonel, MM ana kullanıcısı. |

### 7.2 Tablo: ZTF_STOCK_ITM (Snapshot Kalemleri)
**Birincil Anahtar:** `(snapshot_uuid, item_no)`

| Alan | Anahtar | ABAP Tipi | Örnek | Notlar |
| :--- | :--- | :--- | :--- | :--- |
| **SNAPSHOT_UUID** | X | RAW16 | (ref) | Başlığa yabancı anahtar. |
| **ITEM_NO** | X | NUMC6 | 000010 | Sıralı numara. |
| **MATERIAL_ID** | | CHAR18 | RM-CHOC-001 | Özel malzeme id (MATNR değil). |
| **MATERIAL_DESC** | | CHAR40 | Kakao Tozu | Kısa metin. |
| **STORAGE_LOC** | | CHAR4 | RM01 | Depo yeri. |
| **QTY_ON_HAND** | | DEC(13,3) | 125.500 | Eldeki miktar. |
| **UOM** | | UNIT3 | KG | Ölçü birimi. |
| **REORDER_POINT** | | DEC(13,3) | 150.000 | Eşik değer. |
| **RISK_FLAG** | | CHAR3 | LOW | LOW/OK. |
| **ITEM_VALUE** | | DEC(15,2) | 5600.00 | TRY cinsinden değer. |

### 7.3 Türetilmiş View: ZV_STOCK_RISK (Grafik İçin)
Grafiğe uygun verileri sunmak için bir CDS view kullanılır. LOW olarak işaretlenmiş kalemleri ve güne göre trendi toplar (aggregate).
**Trend penceresi:** Son 7 gün (hareketli).

## 8. UI Spesifikasyonu (Fiori Uygulaması)
**Uygulama Tipi:** Fiori Elements List Report + Object Page.

### 8.1 Seçim / Filtreler
**Zorunlu filtreler:**
*   Üretim Yeri (Varsayılan 1000)
*   Snapshot Tarihi (Varsayılan bugün)

**Opsiyonel filtreler:**
*   Malzeme Tipi
*   Depo Yeri

*Not: Akışı minimal tutmak için, uygulama seçim ekranı olmadan doğrudan en son snapshot'ı gösterecek şekilde de yüklenebilir.*

### 8.2 Liste Raporu (Başlık Listesi)
**Kolonlar:** Üretim Yeri, Snapshot Tarihi, Toplam Kalem, Toplam Değer, Risk Sayısı, Yorum.

---

**Sayfa 6**

**Varsayılan sıralama:** Snapshot Tarihi azalan (descending).

### 8.3 Nesne Sayfası (Snapshot Detayları)
**Bölümler:**
*   **Genel Bakış:** KPI'lar (Toplam Kalem, Toplam Değer, Düşük Stok Sayısı).
*   **Kalem Listesi:** Tablo, düşük stok vurgulanmış.
*   **Grafik:** En düşük 5 kalem + 30 günlük miktar trendi (eğer mevcutsa).

### 8.4 Aksiyonlar
*   **Snapshot Oluştur (Buton):** Verilen Üretim Yeri + Tarih için snapshot yaratır veya üzerine yazar.
*   **CSV Dışa Aktar (Buton):** Kalem listesini dışa aktarır.

## 9. Doğrulamalar ve Hata Yönetimi
**Doğrulamalar:**
*   Snapshot oluşturulurken Üretim Yeri sağlanmalıdır.
*   Snapshot Tarihi gelecekte olmamalıdır.
*   Miktar ve yeniden sipariş noktası >= 0 olmalıdır.
*   Snapshot başına maks kalem: 200 (sert limit).

**Hata yönetimi prensipleri:**
*   Kullanıcı dostu tek bir mesaj göster, teknik detayları uygulama günlüğüne (application log) kaydet.
*   Snapshot oluşturma başarısız olursa, kısmi veri oluşturma (kalemsiz başlık olmamalı).

## 10. Fonksiyonel Olmayan Gereksinimler
*   **Performans:** 500 kaleme kadar Snapshot Oluşturma < 2 saniye.
*   **Güvenlik:** Analistler için Okuma (Read), MM Ana Kullanıcıları için Oluşturma (Generate) erişimi.
*   **Denetim (Audit):** Başlıkta `created_by` ve `created_at` sakla (opsiyonel).

## 11. Kabul Kriterleri (Acceptance Criteria)

*   **AC-01:** Kullanıcı bir snapshot oluşturabilir, başlığı ve kalemleri görebilir.
*   **AC-02:** `QTY_ON_HAND < REORDER_POINT` olduğunda LOW (Düşük) kalemler işaretlenir.
*   **AC-03:** Grafik, En Düşük 5 kalemi ve trendi görüntüler.
*   **AC-04:** CSV Dışa Aktarma doğru şekilde indirilir.
*   **AC-05:** Kapalı Beta'da Snapshot sadece Üretim Yeri 1000'i destekler.

## 12. Açık Noktalar ve Varsayımlar
*   BTP deneme (trial) ortamında ZLI tabloları için veri tohumlama (data seeding) yaklaşımı (manuel yükleme vs oluşturucu sınıfı).

---

**Sayfa 7**

*   Snapshot'ın sonraki fazlarda simüle edilmiş MARA/MARD veri kaynaklarından okuyup okumayacağı.
*   Toplam Değer için para birimi standardını (EUR vs TRY) onaylayın.

## Ek A: Alan Kataloğu

### A.1 Başlık anahtar formatı
**SNAPSHOT_UUID**, UI'da daha kolay gösterim için CHAR32 (hex string) olarak saklanır.

### A.2 Birimler
**UOM** (Ölçü Birimi), Kapalı Beta'da sadece **EA** (Adet) ile sınırlandırılmıştır.
