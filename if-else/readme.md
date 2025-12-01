## 9.  IF-ELIF-ELSE VE KULLANICI ETKİLEŞİMİ

Kodun akışını belirli şartlara göre yönlendirmek için kullanılan karar yapılarıdır. Veri analizinde; bir veriyi belirli aralıklara göre etiketlemek (Örn: Yaş > 65 ise "Emekli") veya hatalı verileri filtrelemek için kullanılır.



#### 1. Mantıksal Hiyerarşi
* **`if`:** Sorgulamanın başladığı bloktur. Koşul `True` ise çalışır, `False` ise atlanır
* **`elif`:** İlk `if` koşulu sağlanmadıysa kontrol edilen ikinci, üçüncü duraktır. Sınırsız sayıda kullanılabilir
* **`else`:** Yukarıdaki hiçbir koşul tutmazsa çalışacak olan "son çare" bloğudur. Koşul belirtilmez

#### 2. `input()` ve Tip Dönüşümü
* **Veri Alma:** Kullanıcıdan terminal üzerinden veri almak için `input("Mesaj")` kullanılır
* **String Tehlikesi:** `input` fonksiyonu her zaman **String (Metin)** döndürür. Matematiksel işlem veya büyüklük/küçüklük kıyaslaması yapılacaksa mutlaka `int()` veya `float()` dönüşümü yapılmalıdır

---

###  Uygulama: İnteraktif Kredi Risk Analiz Simülasyonu

Aşağıdaki kod; kullanıcıdan dinamik olarak veri alan, bu veriyi tip dönüşümüne sokan ve if-elif-else bloklarıyla analiz edip sonuç üreten bir karar destek sistemidir.

```python
# SENARYO: Banka müşterisinin kredi notuna ve gelirine göre kredi onay durumu sorgulama
# Kullanıcıdan veri alınacağı için 'input' fonksiyonu kullanılacak

print("=== KREDİ RİSK ANALİZ SİSTEMİ ===")
print("Lütfen müşteri bilgilerini giriniz.\n")

# --- 1. KULLANICI GİRİŞİ (INPUT & CASTING) ---

# input() string döndürür, sayısal kıyaslama için int() veya float() şarttır
isim = input("Müşteri Adı Soyadı: ")

# Gelir ondalıklı olabilir, float yapıyoruz
aylik_gelir = float(input("Aylık Gelir (TL): ")) 

# Kredi notu tam sayıdır, int yapıyoruz
kredi_notu = int(input("Kredi Notu (0-1900): "))

# --- 2. KARAR MEKANİZMASI (LOGIC) ---

durum = ""
limit = 0

# Mantıksal süzgeç: Önce en zor koşul, sonra diğerleri
if kredi_notu >= 1600:
    durum = "ÇOK YÜKSEK (A+)"
    # Gelirin 5 katı kadar limit
    limit = aylik_gelir * 5
    onay = True

elif kredi_notu >= 1200:
    durum = "İYİ (B)"
    # Gelirin 3 katı kadar limit
    limit = aylik_gelir * 3
    onay = True

elif kredi_notu >= 800:
    durum = "ORTA RİSK (C)"
    # Gelir 20.000 TL üzerindeyse kefille onay, yoksa red
    if aylik_gelir >= 20000:
        limit = aylik_gelir * 1
        onay = True
        durum += " - Kefil Gerekli"
    else:
        onay = False
        
else:
    # 800 altı
    durum = "YÜKSEK RİSK (D)"
    onay = False

# --- 3. SONUÇ RAPORLAMA ---

print("-" * 40)
print(f"SAYIN {isim.upper()}")
print(f"Skor Analizi : {kredi_notu} -> {durum}")

if onay:
    print(f"SONUÇ        : ONAYLANDI")
    print(f"Ön Onay Limit: {limit:.2f} TL")
else:
    print(f"SONUÇ        : REDDEDİLDİ ")
    print("Nedeni       : Kredi puanı veya gelir kriteri yetersiz")

print("-" * 40)

```

# OUTPUT:

```python



=== KREDİ RİSK ANALİZ SİSTEMİ ===
Lütfen müşteri bilgilerini giriniz.

Müşteri Adı Soyadı: Ahmet Yılmaz
Aylık Gelir (TL): 35000
Kredi Notu (0-1900): 1350

----------------------------------------
SAYIN AHMET YILMAZ
Skor Analizi : 1350 -> İYİ (B)
SONUÇ        : ONAYLANDI 
Ön Onay Limit: 105000.00 TL
----------------------------------------
