## 16. HATA YÖNETİMİ (EXCEPTION HANDLING)

Kod yazarken hata almak kaçınılmazdır. Önemli olan bu hataları yönetebilmek ve programın "çökmesini" (crash) engellemektir. 
Veri analizinde binlerce satırlık bir işlem yaparken, tek bir bozuk veri yüzünden tüm sürecin durmasını istemeyiz.

###  Hata Türleri (Exceptions)

Python'da en çok karşılaşacağımız Runtime hataları şunlardır:

#### 1. ZeroDivisionError
* **Tanım:** Bir sayıyı 0'a bölmeye çalıştığımızda oluşur
* **Analiz Senaryosu:** Kar marjı hesaplarken maliyetin 0 olması durumu

#### 2. TypeError 
* **Tanım:** Uyumsuz veri tipleriyle işlem yapmaya çalıştığımızda oluşur
* **Örnek:** `5 + "Elma"` (Sayı ile metin toplanamaz)
* **Analiz Senaryosu:** Excel'den sayı yerine metin gelmesi

#### 3. IndexError (List Index Out of Range)
* **Tanım:** Listede olmayan bir sıradaki elemana ulaşmaya çalıştığımızda oluşur
* **Örnek:** 3 elemanlı bir listenin 10. elemanını istemek `liste[10]`

#### 4. Traceback Okuma Sanatı
* Hata aldığımızda Python bize bir rapor (Traceback) verir
* **Kural:** Her zaman **EN ALT SATIRA** bakılır. Hatanın ne olduğu ve hangi satırda (`line X`) olduğu orada yazar

---

### Try - Except Yapısı (Hata Yakalama)

Programın hata anında durmak yerine, bizim belirlediğimiz alternatif bir rotadan devam etmesini sağlar. `if-else` mantığına benzer ama hatalar içindir.

* **`try`:** "Bunu yapmayı dene..." (Riskli kod buraya yazılır)
* **`except`:** "Eğer hata alırsan programı patlatma, buradaki kodu çalıştır..."
* **`else`:** "Eğer hiç hata çıkmazsa bunu yap..." (Opsiyonel)
* **`finally`:** "Hata olsa da olmasa da en sonda mutlaka bunu yap..." (Veritabanı bağlantısını kapatmak için kullanılır)

---

### Uygulama: Dayanıklı Veri İşleme Hattı

Aşağıdaki kodda; hatalı verilerle dolu bir listeyi, programı çökertmeden işlemeyi simüle ettim.

```python
# SENARYO: Ham veri listesinde 0'a bölünme ve tip hataları var.
# Normalde bu kod ilk hatada dururdu, ama try-except ile devam ettireceğiz.

veriler = [
    {"ad": "Ürün A", "fiyat": 1000, "adet": 10},  # Sorunsuz
    {"ad": "Ürün B", "fiyat": 500,  "adet": 0},   # HATA: ZeroDivisionError (0'a bölünme)
    {"ad": "Ürün C", "fiyat": 200,  "adet": "5"}, # HATA: TypeError (String ile bölme)
    {"ad": "Ürün D", "fiyat": 10000}              # HATA: KeyError (Adet bilgisi yok)
]

print("=== VERİ ANALİZİ BAŞLIYOR ===\n")

basarili_islem = 0
hatali_islem = 0

for urun in veriler:
    urun_adi = urun.get("ad")
    
    try:
        # --- RİSKLİ ALAN ---
        # Burada her an hata çıkabilir
        fiyat = urun["fiyat"]
        adet = urun["adet"]
        
        # Birim fiyat hesabı (Hata potansiyeli yüksek)
        birim_fiyat = fiyat / adet 
        
    except ZeroDivisionError:
        # Adet 0 ise buraya düşer
        print(f"[HATA] {urun_adi}: Adet 0 olamaz! (Sonsuz Döngü Riski)")
        hatali_islem += 1
        
    except TypeError:
        # Adet 'string' ise buraya düşer
        print(f"[HATA] {urun_adi}: Veri tipi hatalı! (Sayı bekleniyor)")
        hatali_islem += 1
        
    except KeyError:
        # Sözlükte anahtar eksikse buraya düşer
        print(f"[HATA] {urun_adi}: Eksik veri! (Adet bilgisi bulunamadı)")
        hatali_islem += 1
        
    except Exception as e:
        # Öngöremediğimiz başka bir hata olursa buraya düşer
        # 'e' değişkeni hatanın orijinal mesajını tutar
        print(f"[BİLİNMEYEN HATA] {urun_adi}: {e}")
        hatali_islem += 1
        
    else:
        # try bloğu sorunsuz çalışırsa burası devreye girer
        print(f"[BAŞARILI] {urun_adi}: Birim Fiyat -> {birim_fiyat:.2f} TL")
        basarili_islem += 1
        
    finally:
        # Hata olsa da olmasa da her döngü sonunda çalışır
        # Genellikle log tutmak veya temizlik yapmak için kullanılır
        print("--- İşlem tamamlandı ---\n")

# SONUÇ RAPORU
print(f"Toplam Başarılı: {basarili_islem}")
print(f"Toplam Hatalı  : {hatali_islem}")
