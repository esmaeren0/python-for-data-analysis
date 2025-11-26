#  LİSTELER (LISTS)

 sayıları veya isimleri depolama,filtreleme,slicing ve manipüle etme. 

---



#### 1. Liste Oluşturma ve Esneklik
* **Karma Veri:** Python listelerine aynı anda hem `int`, hem `str`, hem de `boolean` bulunabilir.
* **İç İçe Listeler (Nested Lists):** Listenin içine liste koyulabiliyor. Bu yapı aslında Excel'deki satır/sütun matris yapısının temeli

#### 2. Parçalama (Slicing) - *Çok Önemli!*
* Listenin tamamını değil, sadece belli bir kısmını almak için `[başlangıç : bitiş]` yapısı kullanılıyor
* `liste[0:3]` -> İlk 3 elemanı getirir
* `liste[::-1]` -> Listeyi kalıcı bozmadan **tersten** okur

#### 3. Listeyi Yönetme (Methodlar)
* `append()` vs `insert()`: Biri hep sona ekliyor (kuyruk), diğeri araya kaynak yapıyor
* `remove()` vs `pop()`: Biri isme göre siliyor (değer odaklı), diğeri konuma göre siliyor (indeks odaklı)
* `len()`: Listenin uzunluğunu (eleman sayısını) verir. Döngülerde çok lazım olacak.
* `in` Operatörü: Bir elemanın listede olup olmadığını sorgulamak için kullanılıyor. `if "Elma" in sepet:` gibi.

---

###  Uygulama: E-Ticaret Sepet Analizi Senaryosu

Aşağıdaki kodda, karışık bir müşteri sepetini analiz edip, hataları düzelttim ve raporladım. Her satıra o an ne düşündüğümü yorum olarak ekledim.

```python
# SENARYO: Bir e-ticaret sitesinin backend'inden gelen ham sepet verisini analiz ediyorum.
# Veri biraz kirli: İçinde stok kodları, ürün isimleri ve hatalı girişler var.

# --- 1. LİSTE OLUŞTURMA (HAM VERİ) ---
# Müşterinin sepeti. Sayı, metin ve boolean (True/False) karışık gelmiş.
sepet = ["Laptop", 15000, "Mouse", 350, "Klavye", "Stok Yok", True]

print("1. Ham Veri Listesi:")
print(sepet)
print(f"Sepetteki Öge Sayısı: {len(sepet)}") # len() fonksiyonu uzunluğu verir.

# --- 2. VERİYE ERİŞİM VE SLICING (DİLİMLEME) ---
# Slicing: Listeden bir kesit alma işlemi.
# İlk ürünü ve fiyatını merak ediyorum (İlk 2 eleman).
ilk_urun_grubu = sepet[0:2] # 0'dan başla, 2'ye kadar git (2 dahil değil!)

# Tersten Bakış: Son eklenen ürünü görmek istiyorum.
son_urun = sepet[-1]

print(f"\n2. İlk Ürün Grubu: {ilk_urun_grubu}")
print(f"Son Eklenen Değer (Boolean): {son_urun}")

# --- 3. LİSTE MANİPÜLASYONU (DÜZELTME) ---

# HATA TESPİTİ: Listede 'Stok Yok' diye bir string var. Bunu kaldırmalıyım
if "Stok Yok" in sepet:
    print("\n-> Uyarı: Sepette stok hatası tespit edildi, temizleniyor...")
    sepet.remove("Stok Yok") # Değeri bulup sildim

# GÜNCELLEME: Müşteri 'Mouse' yerine 'Gaming Mouse' almaya karar verdi
# Mouse'un indeksini bulup değiştireceğim
# Not: Normalde indeksini biliyorsam direkt sepet[2] = ... derdim ama index() ile bulmak daha güvenli
mouse_index = sepet.index("Mouse")
sepet[mouse_index] = "Gaming Mouse"

# Fiyatını da güncelleyelim (Bir sonraki indeks fiyatıydı)
sepet[mouse_index + 1] = 750 

print(f"3. Düzeltilmiş Sepet: {sepet}")

# --- 4. LİSTE BİRLEŞTİRME (EXTEND) ---
# Müşteri "Promosyon Ürünleri" kategorisinden 2 ürün daha ekledi
# Başka bir listeyi, mevcut listeye yapıştırma işlemi
promosyonlar = ["USB Bellek", "Mousepad"]
sepet.extend(promosyonlar) # append yapsaydım liste içinde liste olurdu, extend ile ucuna ekledim

print(f"\n4. Promosyonlar Eklendi: {sepet}")

# --- 5. POP İLE SON İŞLEMİ GERİ ALMA ---
# Müşteri son anda 'Mousepad' almaktan vazgeçti (Listenin en sonundaki eleman)
iade_edilen = sepet.pop() # İndeks vermezsem en sondakini atar

print(f"\n5. İptal Edilen Ürün: {iade_edilen}")
print(f"   Son Durum: {sepet}")

# --- 6. İÇ İÇE LİSTELER (NESTED LISTS - MATRIX MANTIĞI) ---
# Veri analizinde satır ve sütun mantığı buradan geliyor
# [Ürün Adı, Fiyat, Stok Adedi]
envanter = [
    ["Laptop", 15000, 5],
    ["Mouse", 750, 20],
    ["Klavye", 1000, 10]
]

# Sadece Mouse'un stok adedine erişmek istiyorum:
# 1. indeksli listenin (Mouse satırı), 2. indeksli elemanı (Stok sütunu)
mouse_stok = envanter[1][2]

print("-" * 40)
print(f"BONUS: İç İçe Liste Analizi -> Mouse Stok Adedi: {mouse_stok}")


```

### 3. DİĞER LİSTE METODLARI (SIRALAMA, SAYMA, KOPYALAMA)

Listelere sadece veri ekleyip çıkarmıyoruz; çoğu zaman bu veriyi sıraya dizmemiz veya içinde arama yapmamız gerekiyor. Veri analizinde "En yüksek satış hangisi?" veya "Bu hatadan kaç tane var?" sorularını ceavplar


* **`count()` (Sayma):** Listede bir elemanın kaç kez geçtiğini sayar(Frekans bulmak için).
* **`sort()` (Sıralama):** Listeyi küçükten büyüğe (veya A'dan Z'ye) sıralar. **Dikkat:** Listeyi kalıcı olarak değiştirir!
* **`reverse()` (Ters Çevirme):** Listeyi olduğu gibi tersine döndürür (Aynalama yapar).
* **`copy()` (Kopyalama):** Listenin yedeğini alır. **Çok Önemli:** `liste2 = liste1` dersem yedeklemiş olmam, sadece aynı listeye yeni isim takmış olurum. Gerçek yedek için `copy()` şart.
* **`clear()` (Temizleme):** Listenin içini tamamen boşaltır, boş bir liste `[]` haline getirir.

---



```python
# SENARYO: Elimde bir sınıfın sınav notları var.
# Bu notları analiz edip, sıralayıp, istatistik çıkarmak istiyorum

notlar = [45, 90, 60, 45, 100, 75, 45, 85]

print(f"Sınıf Notları (Ham): {notlar}")

# --- 1. COUNT (SAYMA) ---
# Sınıfta kaç kişi 45 alıp dersten kalma sınırında kalmış?
kalan_sayisi = notlar.count(45)
print(f"45 Alıp Kalan Öğrenci Sayısı: {kalan_sayisi}")

# --- 2. COPY (YEDEKLEME) - KRİTİK NOKTA! ---
# Notları sıralamadan önce orijinal sırayı (öğrenci numarasına göre olabilir) kaybetmemek için yedekliyorum.
# HATA UYARISI: 'yedek_notlar = notlar' yapsaydım, orijinali de bozulacaktı!
yedek_notlar = notlar.copy()

# --- 3. SORT & REVERSE (SIRALAMA) ---
# Notları Küçükten Büyüğe Sırala
notlar.sort()
print(f"Sıralı Notlar (Artan): {notlar}")

# Notları Büyükten Küçüğe Sırala (reverse=True parametresi)
notlar.sort(reverse=True)
print(f"Sıralı Notlar (Azalan): {notlar}")

# En yüksek ve en düşük notu artık kolayca görebilirim (İlk ve Son eleman)
print(f"En Yüksek Not: {notlar[0]}")
print(f"En Düşük Not : {notlar[-1]}")

# --- 4. REVERSE (TERSE ÇEVİRME) ---
# Sıralama yapmadan sadece listeyi ters yüz etmek istersem:
# Yedek listeyi kullanalım.
yedek_notlar.reverse()
print(f"Orijinal Listenin Tersten Yazılışı: {yedek_notlar}")

# --- 5. INDEX (KONUM BULMA) ---
# 100 alan öğrenci kaçıncı sırada? (Listede ilk bulduğu konumu verir)
# Not: Listemiz şu an büyükten küçüğe sıralı olduğu için en başta çıkmalı.
derece_index = notlar.index(100)
print(f"100 Alan Öğrencinin Sıralamadaki Yeri (Index): {derece_index}")

# --- 6. CLEAR (TEMİZLEME) ---
# Dönem bitti, listeyi boşaltıp yeni döneme hazırlanalım
notlar.clear()
print(f"Dönem Sonu Liste Durumu: {notlar}")
