# PYTHON İLE VERİ ANALİZİNE GİRİŞ: TEMEL KAVRAMLAR

---

## 1. DEĞİŞKENLER, TİP DÖNÜŞÜMLERİ VE PRINT

**Amaç:**
* **Değişkenler:** Verileri  hafızada tutma
* **Tip Dönüşümleri:** Genellikle dosyalardan (Excel, CSV) metin (string) olarak gelen sayısal verileri, matematiksel işlem yapılabilir hale (int, float) getirmek.
* **Print** 

**Kullanılan Teknikler:**
* `int("5")` -> Metni tam sayıya çevirir.
* `float("12.5")` -> Metni ondalıklı sayıya çevirir.
* `f"Metin {degisken}"` -> Değişkenleri metin içine dinamik olarak yerleştirir.

```python
# SENARYO: Bozuk formatta gelen bir e-ticaret satış verisini analiz etme

# --- 1. DEĞİŞKENLER  ---
# Genellikle veritabanından veya excelden veri  ham gelir
urun_adi = "Kablosuz Kulaklık"
satis_adedi_raw = "150"         
birim_fiyat_raw = "2500.50"      # İşlem yapabilmek için sayıya çevrilmeli

# --- 2. TİP DÖNÜŞÜMLERİ (Veriyi Temizleme) ---
# Matematiksel ciro hesabı yapabilmek için tipleri düzelttim

# '150' metnini integera  çevirme
satis_adedi = int(satis_adedi_raw)

# '2500.50' metnini float yapma
birim_fiyat = float(birim_fiyat_raw)


toplam_ciro = satis_adedi * birim_fiyat

# --- 3. PRINT (Raporlama) ---
# f-string kullanarak temiz ve okunaklı bir rapor çıktısı alma

print("=== GÜNLÜK SATIŞ ANALİZİ ===")
print(f"İncelenen Ürün      : {urun_adi}")
print(f"Ham Veri Tipleri    : Adet({type(satis_adedi_raw).__name__}) - Fiyat({type(birim_fiyat_raw).__name__})")
print(f"Dönüştürülmüş Tipler: Adet({type(satis_adedi).__name__}) - Fiyat({type(birim_fiyat).__name__})")
print("-" * 35)
print(f"Satış Adedi         : {satis_adedi} adet")
print(f"Birim Fiyat         : {birim_fiyat} TL")
print(f"TOPLAM CİRO         : {toplam_ciro} TL")
