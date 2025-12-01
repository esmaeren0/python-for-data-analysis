## 8. DEĞİŞKEN SCOPE VE MANTIKSAL SORGULAR

Programlamada değişkenlerin nerede oluşturulduğu ve nereden erişilebildiği (Scope) hayati önem taşır. Veri analizinde, özellikle sayaçlar veya genel ayarlar yönetilirken bu kurallar devreye girer. Ayrıca Python'ın kendine has "True/False" (Truthy/Falsy) mantığı kodun okunabilirliğini artırır



#### 1. Local vs Global Değişkenler
* **Local Scope:** Bir fonksiyonun **içinde** tanımlanan değişkenlerdir. Sadece o fonksiyon çalışırken hafızada yer kaplar, fonksiyon bitince silinir. Dışarıdan erişilemez
* **Global Scope:** Fonksiyonların **dışında**, ana gövdede tanımlanan değişkenlerdir. Kodun her yerinden okunabilirler

#### 2. `global` Anahtar Kelimesi
* Bir fonksiyon içinde global bir değişkeni sadece **okumak** istiyorsak ekstra bir şeye gerek yoktur
* Ancak fonksiyon içinden global bir değişkenin değerini **değiştirmek** (güncellemek) istiyorsak `global` komutu ile Python'a "Ben içeride yeni bir değişken yaratmıyorum, dışarıdaki değişkeni kullanacağım" dememiz gerekir

#### 3. Pythonic True-False  Sorguları
* Python'da sadece `True` veya `False` değil, boş olan her şey `False`, dolu olan her şey `True` kabul edilir
* `if liste:` yazmak `if len(liste) > 0:` yazmakla aynıdır ve daha profesyoneldir
* **False Sayılanlar:** `0`, `""` (boş string), `[]` (boş liste), `None`, `{}` (boş sözlük)

---

###  Uygulama: Mağaza Kasa ve Stok Takip Simülasyonu

Aşağıdaki kodda; global değişkenleri fonksiyon içinde manipüle etmeyi ve Pythonic yöntemlerle veri kontrolü yapmayı simüle ettim

```python
# SENARYO: Günlük mağaza cirosunun takibi ve hatalı veri girişlerinin kontrolü
# Bu senaryoda 'gunluk_ciro' global olarak yönetilmeli ki her satışta sıfırlanmasın

# --- GLOBAL DEĞİŞKENLER ---
gunluk_ciro = 0          # Kasa (Değiştirilecek)
hatali_islem_sayisi = 0  # Hata Sayacı (Değiştirilecek)
magaza_adi = "TechStore" # Sabit (Sadece okunacak)

# --- FONKSİYONLAR ---

def veri_kontrol(veri):
    """
    Pythonic True/False sorgusu yapar
    Veri boşsa (None, 0, "", []) False döner
    """
    if veri:
        return True
    else:
        return False

def satis_yap(tutar):
    """
    Satış tutarını global ciroya ekler
    Global değişkeni değiştirmek için 'global' keywordü şarttır
    """
    # Python'a dışarıdaki 'gunluk_ciro'yu kullanacağımızı bildiriyoruz
    global gunluk_ciro 
    
    # Eğer global demeseydik, Python burada yeni bir yerel değişken yaratmaya çalışırdı
    gunluk_ciro += tutar 
    print(f"Bilgi: {tutar} TL satış yapıldı. Kasa güncellendi.")

def hatali_islem_kaydet(kayit):
    """
    Hatalı veri geldiğinde sayacı artırır
    """
    global hatali_islem_sayisi
    hatali_islem_sayisi += 1
    print(f"UYARI: '{kayit}' verisi işlenemedi! Toplam Hata: {hatali_islem_sayisi}")

def rapor_ver():
    """
    Global değişkenleri sadece okur, değiştirmez
    Bu yüzden 'global' yazmaya gerek yoktur
    """
    print("-" * 40)
    print(f"MAĞAZA: {magaza_adi}") # Global değişkene direkt erişim (Read-Only)
    print(f"TOPLAM CİRO: {gunluk_ciro} TL")
    print(f"HATA SAYISI: {hatali_islem_sayisi}")
    print("-" * 40)

# --- ANA PROGRAM AKIŞI ---

gelen_islemler = [1500, None, 200, 0, 4500, "", []] # None, 0, "" ve [] geçersiz verilerdir

print("=== SİSTEM BAŞLATILIYOR ===\n")

for islem in gelen_islemler:
    # Pythonic Kontrol: 'if islem:' diyerek verinin dolu olup olmadığına bakıyoruz
    # 0, None veya Boş olanlar False döner
    durum = veri_kontrol(islem)
    
    if durum: # Eğer True ise (Veri geçerliyse)
        satis_yap(islem)
    else:     # Eğer False ise (Veri boş veya 0 ise)
        hatali_islem_kaydet(islem)

# Gün sonu raporunu al
print()
rapor_ver()
```

## OUTPUT :

```python



=== SİSTEM BAŞLATILIYOR ===

Bilgi: 1500 TL satış yapıldı. Kasa güncellendi.
UYARI: 'None' verisi işlenemedi! Toplam Hata: 1
Bilgi: 200 TL satış yapıldı. Kasa güncellendi.
UYARI: '0' verisi işlenemedi! Toplam Hata: 2
Bilgi: 4500 TL satış yapıldı. Kasa güncellendi.
UYARI: '' verisi işlenemedi! Toplam Hata: 3
UYARI: '[]' verisi işlenemedi! Toplam Hata: 4

----------------------------------------
MAĞAZA: TechStore
TOPLAM CİRO: 6200 TL
HATA SAYISI: 4
----------------------------------------
