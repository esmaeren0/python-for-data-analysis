# SUBSTRING (SLICING) DATA ANALİZ PROJELERİ – PYTHON

---

## E-POSTA LİSTESİNDEN DOMAIN ANALİZİ
**Amaç:**
- E-posta adreslerinden kullanıcı adı ve domain kısmını substring ile ayırma
- Hangi domain'den kaç tane var sayma
- En çok kullanılan domain'i hesaplama

**Kullanılan Slicing Teknikleri:**
- `e[:e.index("@")]` → `@` öncesi
- `e[e.index("@") + 1:]` → `@` sonrası

```python
emails = [
    "ali@hotmail.com",
    "zeynep@gmail.com",
    "mehmet@outlook.com",
    "ayse@gmail.com",
    "veli@hotmail.com",
    "deniz@gmail.com",
    "can@yahoo.com"
]

print("=== E-POSTA DOMAIN ANALİZİ ===")

domain_sayac = {}

for e in emails:
    at_index = e.index("@")

    kullanici_adi = e[:at_index]
    domain = e[at_index + 1:]

    print(f"E-posta: {e} | Kullanıcı: {kullanici_adi} | Domain: {domain}")

    if domain in domain_sayac:
        domain_sayac[domain] += 1
    else:
        domain_sayac[domain] = 1

print("\nDomain istatistikleri:")
for d, adet in domain_sayac.items():
    print(f"- {d}: {adet} adet")

en_cok_domain = None
en_cok_adet = 0

for d, adet in domain_sayac.items():
    if adet > en_cok_adet:
        en_cok_adet = adet
        en_cok_domain = d

print(f"\nEn çok kullanılan domain: {en_cok_domain} ({en_cok_adet} adet)\n\n")
