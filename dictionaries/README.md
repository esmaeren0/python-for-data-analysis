## 5. SÖZLÜKLER (DICTIONARIES): ANAHTAR-DEĞER YAPISI

 Listelerde veriye ulaşmak için sıra numarasını bilmeliyim ama Sözlüklerde veriye ismiyle (Key) ulaşabiliyorum. 



* **Tanımlama:** Süslü parantez `{}` kullanılır ve yapı her zaman `Key: Value` şeklindedir
* **Sıra Yoktur:** Sözlüklerde "ilk eleman" diye bir şey yoktur, "Key" vardır
* **JSON İlişkisi:** İnternetten çektiğim verilerin (API) neredeyse hepsi bu formatta geliyor, o yüzden çok önemli
* **Hız:** Binlerce veri olsa bile anahtarı bildiğim an veriyi bulmak milisaniyeler sürüyor
* **Key Kuralları:** Anahtarlar eşsiz olmalı (Tıpkı T.C. Kimlik no gibi), aynı anahtardan iki tane olamaz

---

### Kod Üzerinde Denemelerim (Müşteri Profili Oluşturma)

Bir e-ticaret sitesindeki tek bir müşterinin veritabanındaki kaydını simüle ettim. Pandas tablosunun tek bir satırı gibi düşünebilirim.

```python
# SENARYO: Veritabanından gelen bir müşteri verisini analiz ediyorum
# Bu yapı ileride "JSON" olarak karşıma çıkacakmış

# 1. SÖZLÜK OLUŞTURMA
# Listeden farkı: Her verinin ne olduğu başında yazıyor (Etiketli Veri)
musteri = {
    "id": 1045,
    "ad": "Zeynep",
    "soyad": "Yılmaz",
    "bakiye": 4500.50,
    "uyelik_tipi": "Gold"
}

print(f"Müşteri Kaydı: {musteri}")
print(f"Veri Tipi: {type(musteri)}")

# 2. ELEMAN SEÇME (ERİŞİM YÖNTEMLERİ)

# Yöntem A: Köşeli Parantez ile Erişim (Klasik)
# Anahtarın ismini birebir doğru yazmak zorundayım
isim = musteri["ad"]
bakiye = musteri["bakiye"]

print(f"\nMüşteri: {isim} | Bakiye: {bakiye} TL")

# Yöntem B: .get() Metodu ile Güvenli Erişim
# Eğer 'adres' diye bir anahtar yoksa program hata verip patlamasın diye get kullanılır
# Anahtar yoksa 'None' döndürür veya benim belirlediğim mesajı yazar
adres = musteri.get("adres", "Adres Bilgisi Yok") 
print(f"Adres Durumu: {adres}")

# 3. ELEMAN EKLEMEK
# Listelerdeki gibi append() yok
# Yeni bir anahtar yazıp değer atarsam otomatik ekleniyor
musteri["sehir"] = "İzmir"
musteri["son_giris"] = "2023-10-25"

print(f"\nYeni Bilgiler Eklendi: {musteri}")

# 4. ELEMAN DEĞİŞTİRMEK (GÜNCELLEME)
# Ekleme ile aynı mantık
# Eğer anahtar zaten varsa, Python  bunu güncellemek istiyorsun der ve üzerine yazar
musteri["bakiye"] = 5000.0   # Bakiye arttı
musteri["uyelik_tipi"] = "Platinum" # Üyelik yükseldi

print("-" * 30)
print("=== GÜNCEL MÜŞTERİ KARTI ===")
print(f"İsim: {musteri['ad']} {musteri['soyad']}")
print(f"Şehir: {musteri['sehir']}")
print(f"Yeni Bakiye: {musteri['bakiye']}")
print(f"Statü: {musteri['uyelik_tipi']}")
