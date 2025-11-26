##  TUPLE (DEMETLER): DEĞİŞTİRİLEMEZ LİSTELER


* **Tanımlama:** Köşeli parantez `[]` yerine normal parantez `()` kullanılır
* **Değiştirilemezlik (Immutable):** En kritik özellik bu. Bir kere oluşturduktan sonra içine eleman ekleyemem, silemem veya değiştiremem
* **Hız ve Güvenlik:** Değiştirilemediği için listelere göre daha hızlı çalışır ve verinin yanlışlıkla bozulmasını engeller
* **Tek Eleman Tuzağı:** Tek elemanlı tuple yaparken yanına virgül koymak şart `(5,)` yoksa Python onu matematik işlemi sanıyor
* **Kullanım Alanı:** Veri analizinde asla değişmemesi gereken ayarlar (Veritabanı şifresi, API anahtarları, Enlem/Boylam bilgileri) için kullanılır

---

### Güvenli Veri Saklama

Aşağıda değişmemesi gereken coğrafi koordinatları ve veritabanı ayarlarını Tuple içine hapsettim. Değiştirmeye çalıştığımda Python'ın nasıl hata verdiğini not aldım.

```python
# SENARYO: Bir harita uygulaması için sabit koordinatları ve sunucu ayarlarını tutuyorum
# Bu verilerin program çalışırken yanlışlıkla bile değişmemesi lazım

# 1. TUPLE OLUŞTURMA
# İstanbul'un merkezi ve sunucu bilgileri
konum_istanbul = (41.0082, 28.9784)
server_config = ("192.168.1.1", 8080, "admin_user")

print(f"Konum Verisi: {konum_istanbul}")
print(f"Veri Tipi: {type(konum_istanbul)}")

# 2. ELEMANLARA ERİŞİM (INDEXING)
# Listelerle tamamen aynı mantıkta çalışıyor
enlem = konum_istanbul[0]
boylam = konum_istanbul[1]

print(f"Enlem: {enlem} | Boylam: {boylam}")

# 3. ELEMAN DEĞİŞTİRME DENEMESİ (HATA ALACAĞIM KISIM)
# Listelerde 'server_config[1] = 9090' diyip portu değiştirebilirdim
# Ama Tuple'da bunu yaparsam Python 'TypeError' veriyor
print("Not: Tuple içindeki bir elemanı değiştirmeye çalışma hata döner")

# 4. TUPLE METODLARI (SADECE 2 TANE)
# Listelerdeki gibi append, remove, pop YOK
# Sadece count (say) ve index (bul) var

renkler = ("Kırmızı", "Mavi", "Yeşil", "Mavi", "Siyah")

# Mavi rengi kaç kere geçiyor
mavi_sayisi = renkler.count("Mavi")

# Yeşil rengi kaçıncı sırada
yesil_index = renkler.index("Yeşil")

print(f"Mavi Sayısı: {mavi_sayisi}")
print(f"Yeşil'in Konumu: {yesil_index}")

# 5. UNPACKING (PAKET AÇMA) 
# Tuple içindeki değerleri tek satırda değişkenlere dağıtma
# Veri biliminde fonksiyonlardan birden fazla değer dönerken çok kullanılıyor
ip, port, kullanici = server_config

print("-" * 30)
print(f"IP Adresi : {ip}")
print(f"Port No   : {port}")
print(f"Kullanıcı : {kullanici}")
