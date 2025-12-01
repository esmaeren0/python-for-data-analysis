## NESNE TABANLI PROGRAMLAMA (OOP): SINIFLAR VE NESNELER

 Verileri ve fonksiyonları "Nesne" adı verilen paketlerde toplarız.



#### 1. Class Nedir? (Kalıp/Blueprint)
* Sınıf, nesnelerin nasıl üretileceğini tarif eden **teknik çizim** veya **fabrikadaki kalıptır**
* Tek başına iş yapmaz, sadece bir tanımdır.
* İsimlendirme standardı olarak **PascalCase** (HerKelimeBuyuk) kullanılır. Örn: `VeriSeti`, `MusteriHesabi`

#### 2. Object/Instance Nedir? (Ürün)
* Sınıf kalıbından üretilen **gerçek, yaşayan örnektir**
* Aynı sınıftan (kalıptan) sınırsız sayıda, birbirinden bağımsız nesne üretilebilir
* *Örnekleme (Instantiation):* `nesne = SinifAdi()` komutuyla gerçekleşir

#### 3. Özellikler (Attributes)
* Nesnenin sahip olduğu **değişkenlerdir** (Veri)
* **Sınıf Özelliği (Class Attribute):** Tüm nesnelerde ortak olan sabit değerlerdir (Örn: İnsan sınıfı için `tur = "Homo Sapiens"`)
* **Örnek Özelliği (Instance Attribute):** Her nesneye özel olan değerlerdir (Örn: Ali'nin yaşı, Ayşe'nin boyu). `__init__` içinde tanımlanır

#### 4. Methods
* Sınıfın içine yazılan **fonksiyonlardır** (Davranış)
* Nesnenin yapabildiği eylemleri belirtir (Örn: `kos()`, `veri_temizle()`, `raporla()`)

#### 5. `__init__` ve `self` (En Kritik Nokta)
* **`__init__` (Constructor/Yapıcı):** Sınıftan bir nesne üretildiği anda **otomatik** çalışan ilk metottur. Nesnenin başlangıç ayarlarını (fabrika çıkış ayarları) yapar
* **`self`:** O an üzerinde işlem yapılan nesnenin **kendisini** temsil eder. Python'a "Genel konuşmuyorum, BU nesnenin rengini değiştir" demek için `self.renk` deriz. `self` olmadan nesnenin hafızadaki verilerine erişemeyiz

---

###  Uygulama: Veri Analiz Projesi Yönetim Sistemi

Aşağıdaki kodda; bir yazılım şirketindeki farklı veri analizi projelerini yöneten bir `Proje` sınıfı tasarladım. 


```python
# SENARYO: Şirketteki analiz projelerini standart bir yapıya sokmak için Sınıf oluşturuyoruz

class VeriProjesi:
    # --- 1. SINIF ÖZELLİKLERİ (Class Attributes) ---
    # Bu sınıftan üretilen HER nesne bu bilgiye sahip olacak.
    # Şirket adı her proje için aynıdır, değişmez.
    sirket_adi = "TechData AI Solutions"
    calisma_saatleri = "09:00 - 18:00"

    # --- 2. BAŞLATICI METOD (__init__) ---
    # Nesne ilk oluşturulduğunda (Doğduğunda) çalışır.
    # 'self' parametresi, oluşturulan o yeni nesneyi temsil eder.
    def __init__(self, proje_adi, veri_kaynagi, satir_sayisi):
        # ÖRNEK ÖZELLİKLERİ (Instance Attributes)
        # Her projenin adı, kaynağı ve veri büyüklüğü kendine özeldir.
        self.ad = proje_adi
        self.kaynak = veri_kaynagi
        self.satir = satir_sayisi
        
        # Kullanıcıdan almadığımız ama varsayılan atadığımız özellikler
        self.durum = "Başlangıç Aşamasında"
        self.tamamlanma_yuzdesi = 0
        print(f"-> Yeni Proje Oluşturuldu: {self.ad}")

    # --- 3. ÖRNEK METODLARI (Instance Methods) ---
    
    def bilgi_goster(self):
        """Projenin o anki tüm bilgilerini ekrana döker."""
        print(f"\n--- PROJE KİMLİK KARTI ({self.sirket_adi}) ---")
        print(f"Proje Adı : {self.ad}")
        print(f"Kaynak    : {self.kaynak}")
        print(f"Veri Büy. : {self.satir} satır")
        print(f"Durum     : {self.durum} (%{self.tamamlanma_yuzdesi})")

    def veri_temizle(self):
        """Veri temizleme işlemini simüle eder ve durumu günceller."""
        if self.tamamlanma_yuzdesi < 50:
            print(f"\n... '{self.ad}' için veri temizliği yapılıyor ...")
            # Kendi özelliğini güncellemek için yine 'self' kullanıyoruz
            self.durum = "Veri Temizlendi, Analize Hazır"
            self.tamamlanma_yuzdesi = 50
            # Temizlik sonrası veri azalır (Simülasyon)
            self.satir = self.satir - 100 
        else:
            print(f"\nUYARI: '{self.ad}' zaten temizlenmiş!")

    def analiz_et(self):
        """Analiz işlemini yapar, projeyi bitirir."""
        if self.tamamlanma_yuzdesi >= 50:
            print(f"\n... '{self.ad}' üzerinde Makine Öğrenmesi çalıştırılıyor ...")
            self.durum = "Tamamlandı"
            self.tamamlanma_yuzdesi = 100
        else:
            print("\nHATA: Önce veriyi temizlemelisiniz! (Yüzde 50'nin altında)")

# --- 4. NESNE TÜRETME (INSTANTIATION) ---

# Kalıbı kullanarak 2 farklı proje (nesne) üretiyoruz
# __init__ içindeki parametreleri (ad, kaynak, satir) burada gönderiyoruz
proje1 = VeriProjesi("Müşteri Churn Analizi", "SQL Veritabanı", 15000)
proje2 = VeriProjesi("Satış Tahmini 2025", "Excel Dosyaları", 5000)

# --- 5. METODLARI KULLANMA ---

# Proje 1 üzerinde işlemler
proje1.bilgi_goster()
proje1.veri_temizle()
proje1.analiz_et()
proje1.bilgi_goster() # Son durumu görelim

# Proje 2 üzerinde işlemler (Proje 1'den bağımsızdır)
# Hata kontrolü denemesi: Temizlemeden analize geçmeye çalışalım
proje2.analiz_et()

# output :

-> Yeni Proje Oluşturuldu: Müşteri Churn Analizi
-> Yeni Proje Oluşturuldu: Satış Tahmini 2025

--- PROJE KİMLİK KARTI (TechData AI Solutions) ---
Proje Adı : Müşteri Churn Analizi
Kaynak    : SQL Veritabanı
Veri Büy. : 15000 satır
Durum     : Başlangıç Aşamasında (%0)

... 'Müşteri Churn Analizi' için veri temizliği yapılıyor ...

... 'Müşteri Churn Analizi' üzerinde Makine Öğrenmesi çalıştırılıyor ...

--- PROJE KİMLİK KARTI (TechData AI Solutions) ---
Proje Adı : Müşteri Churn Analizi
Kaynak    : SQL Veritabanı
Veri Büy. : 14900 satır
Durum     : Tamamlandı (%100)

HATA: Önce veriyi temizlemelisiniz! (Yüzde 50'nin altında)
