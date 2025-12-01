## LOOPS VE İLERİ AKIŞ KONTROLÜ

Programlamada tekrarlayan işlemleri otomatize etmek için döngüler kullanılır. 
Veri analizinde milyonlarca satırlık bir veriyi işlemek, tek tek elle değil, 
döngüler aracılığıyla saniyeler içinde yapılır. 



#### 1. `for` Döngüsü (Belirli Tekrar)
* Listeler, demetler (tuples), stringler veya `range()` gibi bir veri grubunun (iterable) üzerinde, eleman sayısı kadar dönen yapıdır.
* **Kullanım:** "Elimdeki listenin sonuna gelene kadar her eleman için şunu yap" mantığıdır.
* **`range(başlangıç, bitiş, adım)`:** Belirli bir sayı aralığında döngü kurmak için kullanılır (Bitiş dahil değildir).

#### 2. `while` Döngüsü (Koşullu Tekrar)
* Belirli bir koşul `True` olduğu sürece sürekli çalışan döngüdür.
* **Risk:** Eğer döngü içindeki koşulu bozacak bir işlem yapılmazsa **Sonsuz Döngü (Infinite Loop)** oluşur ve program kilitlenir.
* **Kullanım:** "İşlem tamamlanana kadar bekle" veya "Dosya inene kadar kontrol et" senaryoları.

#### 3. Döngü Kontrol İfadeleri (`break` & `continue`)
* **`break`:** Döngüyü anında **öldürür** ve sonlandırır. (Örn: Aranan veriyi buldum, gerisine bakmaya gerek yok).
* **`continue`:** Sadece o anki turu **atlar** ve döngünün başına döner. (Örn: Hatalı veriyi geç, sıradakine bak).

---

### Uygulama: Siber Güvenlik Log Analizi ve Tehdit Engelleme

Aşağıdaki kodda; `for` döngüsü ile logları tarayan, `if` ile tehditleri tespit eden, `function` ile bunu modüler yapan ve `while` ile saldırı anında sistemi geçici beklemeye alan komple bir senaryo kurguladım.

```python
# SENARYO: Bir sunucuya gelen istekleri (logları) analiz eden güvenlik duvarı (Firewall) simülasyonu.
# Amaç: Güvenli istekleri kabul et, şüphelileri raporla, saldırı varsa sistemi kilitle.

import time # Bekleme simülasyonu için

# --- FONKSİYONLAR ---

def risk_analizi(ip_adresi, istek_tipi):
    """
    Gelen isteğin risk durumunu analiz eder.
    Returns: 'GÜVENLİ', 'RİSKLİ' veya 'SALDIRI'
    """
    yasakli_tipler = ["SQL_INJECTION", "DDOS_ATTACK", "BRUTE_FORCE"]
    
    if istek_tipi in yasakli_tipler:
        return "SALDIRI"
    elif istek_tipi == "ADMIN_LOGIN":
        return "RİSKLİ"
    else:
        return "GÜVENLİ"

def sistemi_kilitle(saniye):
    """
    Saldırı anında while döngüsü ile geri sayım yaparak sistemi bekletir.
    """
    print(f"\n!!! KRİTİK TEHDİT ALGILANDI !!! Sistem {saniye} saniyeliğine kilitleniyor...")
    sayac = saniye
    while sayac > 0:
        print(f"Sistem koruma modunda... {sayac}")
        sayac -= 1 # Sonsuz döngüden kaçınmak için sayacı azaltıyoruz
        # time.sleep(1) # Gerçek hayatta burada 1 sn beklenir
    print("Sistem tekrar aktif.\n")

# --- ANA VERİ SETİ ---

server_logs = [
    {"ip": "192.168.1.10", "istek": "GET_HOMEPAGE"},   # Güvenli
    {"ip": "192.168.1.12", "istek": "GET_IMAGE"},      # Güvenli
    {"ip": "85.100.22.01", "istek": "ADMIN_LOGIN"},    # Riskli (Dikkat)
    {"ip": "10.0.0.5",     "istek": "GET_CONTACT"},    # Güvenli
    {"ip": "104.22.55.99", "istek": "SQL_INJECTION"},  # SALDIRI (Break çalışacak)
    {"ip": "192.168.1.50", "istek": "GET_ABOUT"}       # Buraya hiç ulaşamayacağız
]

# --- ANA PROGRAM (DÖNGÜ VE KONTROL) ---

print("=== FIREWALL LOG ANALİZİ BAŞLATILIYOR ===")

for log in server_logs:
    mevcut_ip = log["ip"]
    mevcut_istek = log["istek"]
    
    # 1. Adım: Analiz Fonksiyonunu Çağır
    durum = risk_analizi(mevcut_ip, mevcut_istek)
    
    # 2. Adım: Duruma Göre Aksiyon Al (If-Elif-Else)
    if durum == "GÜVENLİ":
        # Güvenli ise ekrana basma, pas geç (Log kirliliğini önle)
        # continue: Döngünün bu adımını bitir, sonraki elemana geç
        continue 
        
    elif durum == "RİSKLİ":
        print(f"[UYARI] Şüpheli işlem tespit edildi: {mevcut_ip} -> {mevcut_istek}")
        
    elif durum == "SALDIRI":
        print(f"[ALARM] SALDIRI ENGELLENDİ: {mevcut_ip} IP adresi bloklandı!")
        # Saldırı varsa analizi hemen kesmemiz lazım
        sistemi_kilitle(3) # While döngüsünü tetikle
        break # Döngüyü tamamen sonlandır (Diğer loglara bakma)

print("=== GÜVENLİK TARAMASI SONLANDIRILDI 

#output-----------------------------------


=== FIREWALL LOG ANALİZİ BAŞLATILIYOR ===
[UYARI] Şüpheli işlem tespit edildi: 85.100.22.01 -> ADMIN_LOGIN
[ALARM] SALDIRI ENGELLENDİ: 104.22.55.99 IP adresi bloklandı!

!!! KRİTİK TEHDİT ALGILANDI !!! Sistem 3 saniyeliğine kilitleniyor...
Sistem koruma modunda... 3
Sistem koruma modunda... 2
Sistem koruma modunda... 1
Sistem tekrar aktif.

=== GÜVENLİK TARAMASI SONLANDIRILDI ===
