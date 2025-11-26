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
