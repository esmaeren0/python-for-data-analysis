## 7.FUNCTIONSVE MODÜLER YAPI

Fonksiyonlar, karmaşık işlemleri parçalara bölerek yönetilebilir hale getiren ve kod tekrarını engelleyen yapılardır. 
Veri analizinde ETL (Extract, Transform, Load) süreçlerini otomatize etmek için kullanılır

#### 1. Fonksiyon Yapısı ve `def`
* **Tanımlama:** `def` anahtar kelimesi ile başlar ve fonksiyon içeriği girintili (indent) yazılır
* **Parametre:** Fonksiyonun çalışırken dışarıdan ihtiyaç duyduğu değişkenler
* **Docstring:** Fonksiyonun ne iş yaptığını anlatan ve `""" """` arasına yazılan dokümantasyon notu

#### 2. `return` ve `print` Ayrımı
* **`print()`:**
* **`return`:** Fonksiyonun ürettiği değeri programın akışına geri döndürür, bu değer bir değişkene atanabilir

#### 3. Default Argümanlar
* Parametreye değer gönderilmediğinde devreye giren standart değer
* Tanımlama sırasında varsayılan değeri olan parametreler her zaman **en sona** yazılmalıdır

---

### Uygulama: E-Ticaret Veri Ön İşleme Hattı


```python
# SENARYO: Ham satış verilerinin temizlenmesi, hesaplanması ve raporlanması

# --- 1. FONKSİYON TANIMLAMALARI ---

def veri_temizle(metin):
    """
    Metindeki gereksiz boşlukları siler ve küçük harfe çevirir
    Input: "  LaPtoP " -> Output: "laptop"
    """
    return metin.strip().lower()

def kdv_hesapla(fiyat, kdv_orani=0.20):
    """
    Fiyat üzerinden KDV dahil tutarı hesaplar
    Default Değer: Oran girilmezse %20 (0.20) alınır
    """
    return fiyat * (1 + kdv_orani)

def kar_analizi(satis_fiyati, maliyet):
    """
    Satış ve maliyet farkına göre durum analizi yapar
    Sonucu metin (string) olarak return eder
    """
    kar = satis_fiyati - maliyet
    if kar > 0:
        return "KARLI SATIŞ"
    elif kar == 0:
        return "NÖTR (BAŞA BAŞ)"
    else:
        return "ZARAR"

def raporla(urun_adi, son_fiyat, durum):
    """
    Sonuçları ekrana basan fonksiyon (Void)
    Return kullanmaz, sadece çıktı verir
    """
    print(f"| {urun_adi.ljust(15)} | {str(son_fiyat).ljust(10)} | {durum} |")

# --- 2. ANA PROGRAM AKIŞI ---

# Veritabanından gelen örnek ham veri seti
satis_verileri = [
    {"urun": "  LaPtoP ", "fiyat": 15000, "maliyet": 12000},       # Default oran (%20)
    {"urun": "MOUSE", "fiyat": 500, "maliyet": 750},               # Zarar durumu
    {"urun": "   klavye", "fiyat": 1000, "maliyet": 800, "vergi": 0.10} # Özel oran (%10)
]

print("=== GÜNLÜK SATIŞ ANALİZ RAPORU ===")
print("-" * 50)
print(f"| {'ÜRÜN'.ljust(15)} | {'FİYAT'.ljust(10)} | {'DURUM'} |")
print("-" * 50)

for kayit in satis_verileri:
    # 1. Adım: Veri Temizliği
    temiz_isim = veri_temizle(kayit["urun"])
    
    # 2. Adım: Hesaplama (Varsayılan argüman kontrolü)
    ozel_vergi = kayit.get("vergi")
    
    if ozel_vergi:
        son_fiyat = kdv_hesapla(kayit["fiyat"], ozel_vergi)
    else:
        son_fiyat = kdv_hesapla(kayit["fiyat"]) # Default 0.20 devreye girer
        
    # 3. Adım: Analiz ve Raporlama
    analiz_sonucu = kar_analizi(kayit["fiyat"], kayit["maliyet"])
    raporla(temiz_isim, son_fiyat, analiz_sonucu)

print("-" * 50)


=== GÜNLÜK SATIŞ ANALİZ RAPORU ===
--------------------------------------------------
| ÜRÜN            | FİYAT      | DURUM |
--------------------------------------------------
| laptop          | 18000.0    | KARLI SATIŞ |
| mouse           | 600.0      | ZARAR |
| klavye          | 1100.0     | KARLI SATIŞ |
--------------------------------------------------
