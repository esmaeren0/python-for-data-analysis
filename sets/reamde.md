## 6. SETS (KÜMELER)

Venn Şeması, Veri analizinde binlerce satırlık verideki "Tekrar eden" (Duplicate) kayıtları tek komutla temizlemek için en iyi yöntem bu.

**Öğrendiklerim ve Kısa Notlar:**

* **Unique:** Bir küme içinde aynı elemandan asla iki tane olamaz. Listeyi kümeye çevirdiğim an tüm kopyalar silinir
* **Sırasızlık (Unordered):** İndeks numarası yoktur. `küme[0]` diyemem çünkü elemanların yeri sabit değildir
* **Matematiksel Güç:** İki veri seti arasındaki farkı veya ortak noktaları bulmak için döngü yazmama gerek kalmıyor, tek komut yetiyor
* **Hız:** Listede "bu eleman var mı" diye aramak yavaşken, kümede (Set) bu işlem şimşek hızında

---

###  CRM Müşteri Analizi

İki farklı pazarlama kampanyasından gelen müşteri listelerini karşılaştırdım. Kimler ortak, kimler sadece tek bir kampanyada var, bunları analiz ettim.

```python
# SENARYO: Elimde iki farklı Excel listesinden gelen müşteri isimleri var
# Bazı müşteriler iki listede de olabilir, amacım bunları ayrıştırmak

# 1. SET OLUŞTURMA VE DUPLICATE  TEMİZLİĞİ
# İlk listede 'Ahmet' ismi yanlışlıkla iki kere yazılmış
kampanya_A_listesi = ["Ahmet", "Mehmet", "Ayşe", "Ahmet", "Zeynep"]

# Listeyi Set'e çevirdiğim an tekrarlar otomatik siliniyor
kampanya_A = set(kampanya_A_listesi)

# İkinci kümeyi direkt oluşturuyorum
kampanya_B = {"Ayşe", "Fatma", "Hayri", "Mehmet"}

print(f"Tekrarlar Temizlendi (A): {kampanya_A}")
# Çıktıda 'Ahmet' tek bir tane kaldı

# 2. ELEMAN EKLEME & ÇIKARMA
# Yeni bir müşteri geldi
kampanya_A.add("Burak")

# Bir müşteri listeden çıkmak istedi
# remove() yerine discard() kullandım, çünkü isim yoksa discard hata vermiyor
kampanya_A.discard("Zeynep") 

print(f"Güncel A Listesi: {kampanya_A}")

# 3. KESİŞİM (INTERSECTION) - ORTAK MÜŞTERİLER
# "Her iki kampanyaya da katılan sadık müşteriler kimler?"
ortak_musteriler = kampanya_A.intersection(kampanya_B)
# Kısa yol operatörü: kampanya_A & kampanya_B

print(f"\nHer İki Listede Olanlar (Kesişim): {ortak_musteriler}")

# 4. FARK İŞLEMLERİ 
# "Sadece A kampanyasında olup, B'de OLMAYANLAR kimler?"
sadece_A = kampanya_A.difference(kampanya_B)
# Kısa yol operatörü: kampanya_A - kampanya_B

print(f"Sadece A'da Olanlar (Fark): {sadece_A}")

# 5. BİRLEŞİM (UNION) - TOPLAM TEKİL MÜŞTERİ
# "Elimde toplam kaç tane BENZERSİZ kişi var?" (Kopyaları almadan birleştirir)
tum_musteriler = kampanya_A.union(kampanya_B)
# Kısa yol operatörü: kampanya_A | kampanya_B

print(f"Toplam Tekil Kişi Listesi: {tum_musteriler}")

# 6. SORGU İŞLEMLERİ
# Mehmet bu listede var mı?
kontrol = "Mehmet" in tum_musteriler
print(f"\nMehmet listede mi?: {kontrol}")

# Alt küme sorgusu: Ortak müşteriler, A kümesinin bir parçası mı?
alt_kume_mi = ortak_musteriler.issubset(kampanya_A)
print(f"Ortak olanlar A'nın alt kümesi mi?: {alt_kume_mi}")
