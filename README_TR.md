# 🏭 SourcAP Kapalı Beta — Simülasyon: Stok Özeti (RAP)

[🇬🇧 For English Version click here](./README.md)

**SourcAP Kapalı Beta Resmi Şablon Reposu.**
Bu repo, simülasyon göreviniz için başlangıç noktası ve talimatları içerir.

## 🚀 Simülasyonu Nasıl Tamamlayacaksınız?

Çözümünüzü teslim etmek için aşağıdaki adımları izleyin:

### 1. 📖 Fonksiyonel Dökümanı Okuyun
İnşa etmeniz gereken her şey (tablolar, objeler, mantık) **Fonksiyonel Spesifikasyon Dökümanı** içinde tanımlanmıştır.
👉 **[Fonksiyonel Dökümanı Oku (PDF)](./docs/Toffy_Functional_Spec_TR.pdf)**

*Not: Bu döküman "Toffy Manufacturing" gereksinimlerini içerir.*

### 2. 🛠️ Ortam ve Proje Kurulumu
Kodlamadan önce ABAP ortamınızın hazır olduğundan emin olun.
*   ADT'yi (Eclipse) açın.
*   Yeni bir **ABAP Cloud Project** oluşturun.
*   `Z_BETA_STOCK_<ADINIZ>` isminde bir paket oluşturun.
*   [Kurulum Rehberine Bak](./docs/01-environment-setup.md)

### 3. 💻 Uygulama (Implementation)
Fonksiyonel Dökümanı kullanarak şunları oluşturun:
*   **Veritabanı Tablosu**: Stok verisini tutacak tablo.
*   **CDS View'lar**: Interface ve Consumption view'ları.
*   **Behavior Definition**: Read-only (veya standart).
*   **Service**: Service Definition ve Binding.

### 4. 📤 SourcAP Üzerinden Teslim
Kodlarınızı kendi private (gizli) GitHub reponuza yükledikten sonra:
1.  **SourcAP Platformuna** gidin.
2.  **Simülasyon Teslim** alanına ilerleyin.
3.  **Repo URL**'nizi yapıştırın.
4.  Son **Commit Hash**'inizi yapıştırın.

---
**Yardım mı lazım?** [SSS](./docs/04-faq.md) (FAQ) bölümüne bakın.
