# IT Teknik Sözlük

## 1. Projenin Amacı

Bu proje, IT ve teknoloji alanında karşılaşılan ancak anlamı bilinmeyen veya yeterince hakim olunmayan kavramların **konu bazlı, temel seviyeden ileri seviyeye doğru ve birbirleriyle ilişkileri gösterilerek** öğrenilebilmesi amacıyla oluşturulmaktadır.

Bu proje klasik bir alfabetik sözlük değildir.

Amaç yalnızca:

> "X nedir?"

sorusuna kısa bir cevap vermek değil; X kavramının ait olduğu teknoloji alanını, çalışma mantığını, diğer teknolojilerle ilişkisini, kullanım alanlarını, avantajlarını, dezavantajlarını ve hangi durumlarda tercih edildiğini anlaşılır şekilde açıklamaktır.

---

# 2. Sözlüğün Organizasyon Mantığı

Kavramlar alfabetik olarak sıralanmayacaktır.

Kavramlar ait oldukları **ana teknoloji alanı → alt alan → konu → kavram** hiyerarşisine göre organize edilecektir.

Örneğin:

```text
IT Teknik Sözlük
│
├── Sistem
│   ├── Depolama
│   │   ├── HDD
│   │   ├── SSD
│   │   ├── SATA SSD
│   │   ├── NVMe SSD
│   │   └── SAS
│   │
│   ├── İşletim Sistemleri
│   ├── CPU
│   ├── RAM
│   └── Virtualization
│
├── Network
│   ├── Temel Kavramlar
│   ├── Switching
│   ├── Routing
│   ├── DNS
│   ├── DHCP
│   ├── BGP
│   ├── OSPF
│   └── VXLAN
│
├── Güvenlik
│   ├── Endpoint Security
│   │   ├── Antivirus
│   │   ├── EPP
│   │   └── EDR
│   │
│   ├── Network Security
│   ├── Identity Security
│   ├── Zero Trust
│   └── SIEM
│
├── Cloud
│   ├── IaaS
│   ├── PaaS
│   ├── SaaS
│   ├── Container
│   └── Kubernetes
│
├── DevOps
├── Database
├── Web
├── Storage
└── Monitoring
```

Bu yapı kavramların birbirleriyle ilişkilerinin anlaşılmasını kolaylaştırmalıdır.

---

# 3. Yeni Bir Kavram Eklendiğinde

Kullanıcı örneğin:

```text
NVMe nedir?
```

diye sorduğunda yalnızca NVMe'nin tanımı verilmemelidir.

Öncelikle kavramın hangi kategoriye ait olduğu belirlenmelidir.

Örneğin:

```text
Sistem
└── Storage
    └── Disk Teknolojileri
        └── NVMe
```

Daha sonra konu en temel seviyeden başlanarak açıklanmalıdır.

Benzer şekilde:

```text
EDR nedir?
```

sorusu:

```text
Güvenlik
└── Endpoint Security
    └── Endpoint Detection and Response
        └── EDR
```

şeklinde konumlandırılabilir.

Kullanıcı tarafından kullanılan kavramın hangi kategoriye ait olduğu kesin değilse, mevcut sözlük yapısına en uygun kategori belirlenmeli ve gerekirse yeni bir alt kategori oluşturulmalıdır.

---

# 4. Anlatım Prensibi

Her konu **en basit seviyeden başlayıp gerektiği kadar derinleşmelidir.**

Anlatım sırası mümkün olduğunca şu mantığı takip etmelidir:

```text
Kavram nedir?
      ↓
Neden ortaya çıkmıştır?
      ↓
Hangi problemi çözer?
      ↓
Nasıl çalışır?
      ↓
Temel bileşenleri nelerdir?
      ↓
Diğer teknolojilerle ilişkisi nedir?
      ↓
Alternatifleri nelerdir?
      ↓
Alternatiflerinden farkı nedir?
      ↓
Avantajları
      ↓
Dezavantajları
      ↓
Nerelerde kullanılır?
      ↓
Ne zaman tercih edilir?
      ↓
Gerçek hayattan örnek
      ↓
İleri seviye detaylar
```

Ancak her kavram için bütün başlıkların zorunlu olarak doldurulması gerekmez.

Konuya anlamlı katkı sağlamayan başlıklar kullanılmamalıdır.

---

# 5. Her Kavram İçin İçerik Standardı

Her kavram için mümkün olduğunca aşağıdaki yapı kullanılmalıdır.

## 5.1. Tanım

Kavramın herkesin anlayabileceği şekilde kısa ve net tanımı.

İlk paragrafta teknik detaylara boğulmadan:

> "Bu nedir?"

sorusunun cevabı verilmelidir.

---

## 5.2. Neden Gereklidir?

Kavramın hangi ihtiyacı karşılamak için ortaya çıktığı açıklanmalıdır.

Mümkünse kavramın olmadığı durumda yaşanacak problem de gösterilmelidir.

---

## 5.3. Hangi Problemi Çözer?

Kavramın teknik olarak hangi problemi çözdüğü açıklanmalıdır.

---

## 5.4. Nasıl Çalışır?

Çalışma mantığı temel seviyeden başlayarak açıklanmalıdır.

Gerekirse:

- Paket akışı
- Veri akışı
- İşlem sırası
- Bileşenler
- Protokoller
- Kontrol düzlemi
- Veri düzlemi
- İşletim sistemi davranışı
- Donanım davranışı

gibi detaylar kullanılabilir.

---

## 5.5. Temel Bileşenler

Kavramın anlaşılması için bilinmesi gereken alt bileşenler açıklanmalıdır.

---

## 5.6. Benzer Teknolojilerle Karşılaştırma

Kavramın alternatifleri veya benzer teknolojileri varsa mutlaka karşılaştırılmalıdır.

Örneğin:

| Özellik | HDD | SATA SSD | NVMe SSD |
|---|---|---|---|
| Teknoloji | Mekanik | NAND Flash | NAND Flash |
| Arabirim | SATA | SATA | PCIe |
| Hareketli parça | Var | Yok | Yok |
| Gecikme | Yüksek | Düşük | Çok düşük |
| Performans | Düşük | Orta/Yüksek | Yüksek/Çok yüksek |
| Kullanım | Arşiv, düşük maliyet | Genel kullanım | Performans gerektiren sistemler |

Tablolar yalnızca gerçekten karşılaştırma yapmayı kolaylaştırdığı durumlarda kullanılmalıdır.

---

## 5.7. Avantajları

Kavramın sağladığı avantajlar maddeler halinde açıklanmalıdır.

---

## 5.8. Dezavantajları

Kavramın sınırlamaları ve dezavantajları açıklanmalıdır.

---

## 5.9. Nerelerde Kullanılır?

Gerçek dünyadaki kullanım alanları açıklanmalıdır.

Örneğin:

- Veri merkezleri
- Kurumsal sistemler
- Son kullanıcı sistemleri
- Cloud
- Network
- Güvenlik
- Sanallaştırma
- Kubernetes

gibi alanlarla ilişkisi varsa belirtilmelidir.

---

## 5.10. Ne Zaman Tercih Edilir?

Kavramın hangi senaryolarda tercih edilmesinin anlamlı olduğu açıklanmalıdır.

Mümkünse:

```text
Eğer X ihtiyacı varsa → Y tercih edilebilir.

Eğer Z ihtiyacı varsa → Alternatif teknoloji daha uygun olabilir.
```

şeklinde pratik karar mantığı verilmelidir.

---

## 5.11. Gerçek Hayattan Örnek

Kavramın gerçek bir IT altyapısında nasıl kullanılabileceği örneklenmelidir.

Örnekler mümkün olduğunca gerçek sistem mimarilerine yakın olmalıdır.

---

## 5.12. Diğer Konularla İlişkisi

Kavramın sözlükteki diğer konularla ilişkisi gösterilmelidir.

Örneğin:

```text
NVMe
 ├── PCIe
 ├── SSD
 ├── NAND Flash
 ├── Storage
 ├── Server
 └── Virtualization
```

Bu bölüm özellikle birbirine bağlı teknolojilerin öğrenilmesi açısından önemlidir.

---

## 5.13. Bilinmesi Gereken İlgili Kavramlar

Konu anlaşılırken karşılaşılabilecek diğer teknik terimler listelenebilir.

Örneğin NVMe için:

- PCIe
- NAND
- SSD
- IOPS
- Throughput
- Latency
- Queue Depth
- NVMe-oF

---

# 6. Teknik Seviyenin Belirlenmesi

İçerik yalnızca başlangıç seviyesinde kalmamalıdır.

Anlatım mümkün olduğunca üç katmanda ilerlemelidir:

### Temel Seviye

Kavramı ilk defa gören kişinin anlayabileceği açıklama.

### Orta Seviye

Çalışma mantığı, bileşenleri ve diğer teknolojilerle ilişkileri.

### İleri Seviye

Gerekiyorsa:

- Protokol detayları
- Paket/çerçeve yapıları
- Kontrol düzlemi
- Veri düzlemi
- Performans
- Ölçeklenebilirlik
- Failure senaryoları
- Güvenlik
- Operasyonel detaylar
- Gerçek dünya mimarileri

açıklanmalıdır.

Her konu için ileri seviye detay zorunlu değildir. Ancak teknik olarak önemli olan ayrıntılar atlanmamalıdır.

---

# 7. Terminoloji

İngilizce teknik terimler mümkün olduğunca korunmalıdır.

Örneğin:

```text
Endpoint Detection and Response (EDR)
Virtual Private Network (VPN)
Domain Name System (DNS)
Network Interface Card (NIC)
```

İlk kullanımda:

```text
İngilizce terim (Türkçe açıklama)
```

formatı tercih edilmelidir.

Yaygın teknik terimler gereksiz şekilde Türkçeleştirilmemelidir.

Örneğin:

```text
switch
router
firewall
VLAN
VTEP
overlay
underlay
endpoint
server
storage
```

gibi terimler teknik bağlama uygun şekilde kullanılabilir.

---

# 8. Ürün ve Üretici Bağımlılığı

Bir teknoloji anlatılırken mümkün olduğunca ürün veya üretici bağımsız anlatım yapılmalıdır.

Örneğin:

```text
EDR
```

önce genel teknoloji olarak anlatılmalıdır.

Daha sonra gerekiyorsa:

```text
CrowdStrike
Microsoft Defender
SentinelOne
Trellix
```

gibi ürün örnekleri verilebilir.

Aynı şekilde:

```text
BGP
```

önce standart/protokol olarak açıklanmalı, ardından:

```text
Cisco
Huawei
Juniper
Arista
FRR
```

gibi implementasyon örnekleri gerektiğinde eklenmelidir.

---

# 9. Komut ve Konfigürasyonlar

Bir teknolojinin anlaşılması için CLI veya konfigürasyon örneği faydalıysa kullanılmalıdır.

Örneğin:

```bash
show ip bgp summary
```

veya:

```text
display bgp peer
```

gibi örnekler verilebilir.

Ancak komutlar kavramın kendisinin önüne geçmemelidir.

Önce:

```text
Bu teknoloji nedir?
Nasıl çalışır?
```

soruları cevaplanmalı, daha sonra:

```text
Bu sistemde nasıl görülür / yapılandırılır?
```

gösterilmelidir.

---

# 10. Diyagramlar

Bir konunun görsel olarak daha iyi anlaşılabileceği durumlarda Mermaid diyagramları veya uygun görseller kullanılabilir.

Örneğin:

```mermaid
flowchart LR
    Client --> DNS
    DNS --> Server
```

Diyagramlar yalnızca görsel amaçlı değil, kavramın çalışma mantığını açıklamak amacıyla kullanılmalıdır.

---

# 11. Sözlük İçeriğinin GitHub'a Hazır Olması

## ÖNEMLİ KURAL

Kullanıcı bir kavramın veya konunun işlenmesini istediğinde oluşturulan **sözlük bölümü doğrudan GitHub reposundaki ilgili `.md` dosyasına kopyalanıp yapıştırılabilecek nihai Markdown formatında hazırlanmalıdır.**

Çıktı:

- Ek açıklama içermemelidir.
- "İşte hazırladım" gibi ifadeler içermemelidir.
- ChatGPT'ye yönelik açıklamalar içermemelidir.
- Kullanıcıya yönelik talimatlar içermemelidir.
- Taslak veya placeholder içermemelidir.
- Gereksiz sohbet metni içermemelidir.
- Markdown formatında olmalıdır.
- Başlık hiyerarşisi düzgün olmalıdır.
- Kod blokları doğru Markdown formatında olmalıdır.
- Tablolar GitHub Markdown formatında olmalıdır.
- Mermaid kullanılıyorsa GitHub tarafından desteklenen format kullanılmalıdır.

Yani kullanıcı:

```text
NVMe nedir?
```

dediğinde çıktı doğrudan örneğin:

```markdown
# NVMe

## Tanım

...

## Neden Gereklidir?

...

## Nasıl Çalışır?

...

## NVMe ve Diğer Disk Teknolojileri

| Özellik | HDD | SATA SSD | NVMe |
|---|---|---|---|
| ... | ... | ... | ... |

...
```

şeklinde hazırlanmalıdır.

Kullanıcı bu çıktıyı **ek bir düzenleme yapmadan GitHub'daki ilgili `.md` dosyasına koyabilmelidir.**

---

# 12. Konu Başlığı ile Sözlük İçeriğinin Ayrılması

Bir kavram işlenirken iki farklı seviyedeki bilgi birbirinden ayrılmalıdır:

### Kategori / Konum

Kavramın sözlükte nerede bulunduğunu belirtir.

Örneğin:

```text
Sistem
└── Storage
    └── Disk Teknolojileri
```

### Sözlük İçeriği

GitHub'a konulacak asıl teknik içeriktir.

Örneğin:

```markdown
# NVMe

## Tanım

NVMe...
```

Kategori bilgisi, sözlük maddesinin içine gereksiz şekilde tekrar edilmemelidir.

---

# 13. Dosya ve Klasör Yapısı

Örnek repository yapısı:

```text
it-technical-dictionary/
│
├── README.md
│
├── sistem/
│   ├── cpu/
│   ├── ram/
│   ├── storage/
│   │   ├── hdd.md
│   │   ├── ssd.md
│   │   ├── sata-ssd.md
│   │   ├── nvme.md
│   │   └── sas.md
│   │
│   ├── operating-systems/
│   └── virtualization/
│
├── network/
│   ├── temel-kavramlar/
│   ├── switching/
│   ├── routing/
│   ├── dns/
│   ├── dhcp/
│   ├── bgp/
│   ├── ospf/
│   └── vxlan/
│
├── guvenlik/
│   ├── endpoint-security/
│   │   ├── antivirus.md
│   │   ├── epp.md
│   │   └── edr.md
│   │
│   ├── network-security/
│   ├── identity/
│   ├── zero-trust/
│   └── siem/
│
├── cloud/
├── devops/
├── database/
├── web/
├── monitoring/
└── storage/
```

Gerçek proje büyüdükçe klasör yapısı gerektiği şekilde genişletilebilir.

---

# 14. Kavramlar Arası Linkleme

Bir kavram başka bir kavramın anlaşılması için önemliyse GitHub Markdown bağlantısı kullanılmalıdır.

Örneğin:

```markdown
NVMe, [PCIe](../pcie.md) üzerinden çalışan bir storage protokolüdür.
```

Bu sayede sözlük yalnızca birbirinden bağımsız sayfalardan oluşan bir yapı değil, birbirine bağlı bir **IT bilgi ağı** haline gelir.

---

# 15. Yeni Bir Kategori Oluşturma

Bir kavram mevcut kategorilerden hiçbirine mantıklı şekilde yerleştirilemiyorsa yeni kategori oluşturulabilir.

Ancak aynı veya çok benzer kategorilerin gereksiz şekilde çoğaltılmasından kaçınılmalıdır.

Örneğin:

```text
Security
Network Security
Network Security Technologies
Network Security Tools
```

gibi gereksiz kategori çoğaltmaları yapılmamalıdır.

Kategori yapısı mümkün olduğunca sade ve sürdürülebilir tutulmalıdır.

---

# 16. Aynı Kavramın Farklı Alanlardaki Kullanımı

Bir terim birden fazla teknoloji alanında kullanılıyorsa kavramın bağlama göre anlamı açıklanmalıdır.

Örneğin:

```text
Overlay
```

Network alanında farklı,

```text
Storage Overlay
```

veya başka bir teknoloji alanında farklı anlamlara gelebilir.

Bu durumda kavramın hangi bağlamda ele alındığı açıkça belirtilmelidir.

---

# 17. Karşılaştırma Tabloları

Birbirine alternatif veya benzer teknolojiler olduğunda karşılaştırma tablosu kullanılmalıdır.

Örneğin:

| Özellik | Teknoloji A | Teknoloji B | Teknoloji C |
|---|---|---|---|
| Kullanım amacı | | | |
| Performans | | | |
| Ölçeklenebilirlik | | | |
| Maliyet | | | |
| Karmaşıklık | | | |
| Avantaj | | | |
| Dezavantaj | | | |
| Tipik kullanım alanı | | | |

Tablo, yalnızca gerçekten anlamlı bir karşılaştırma sağlıyorsa kullanılmalıdır.

---

# 18. Gerçek Dünya Perspektifi

Teknik açıklamalarda yalnızca teorik bilgi verilmemelidir.

Uygun olduğu durumlarda:

- Enterprise
- Data Center
- ISP
- Cloud
- Kubernetes
- Security Operations
- Network Operations
- End User Computing
- Storage
- Virtualization

gibi gerçek IT ortamlarından örnekler verilmelidir.

Amaç okuyucunun:

> "Bu teknoloji gerçek hayatta nerede karşıma çıkar?"

sorusunun cevabını anlayabilmesidir.

---

# 19. Güncellik

Teknolojiler zaman içinde değişebilir.

Özellikle:

- Ürün sürümleri
- Protokol özellikleri
- Cloud servisleri
- Güvenlik ürünleri
- İşletim sistemleri
- Vendor özellikleri

gibi zamanla değişebilen bilgiler verilirken güncel bilgi tercih edilmelidir.

Standart veya temel teknik bilgiler ile sürüme/vendor'a bağlı bilgiler birbirinden ayrılmalıdır.

---

# 20. Kalite Kontrol

Bir kavram GitHub'a eklenmeden önce aşağıdaki sorular kontrol edilmelidir:

- [ ] Kavram doğru kategoriye yerleştirildi mi?
- [ ] Kavramın kısa ve net tanımı var mı?
- [ ] Neden gerekli olduğu açıklanmış mı?
- [ ] Hangi problemi çözdüğü anlatılmış mı?
- [ ] Çalışma mantığı açıklanmış mı?
- [ ] Gerekli temel kavramlar açıklanmış mı?
- [ ] Benzer teknolojilerle farkı belirtilmiş mi?
- [ ] Avantajları belirtilmiş mi?
- [ ] Dezavantajları belirtilmiş mi?
- [ ] Kullanım alanları verilmiş mi?
- [ ] Gerçek dünya örneği verilmiş mi?
- [ ] İlgili diğer kavramlara bağlantı verilmiş mi?
- [ ] Gerekiyorsa karşılaştırma tablosu eklenmiş mi?
- [ ] Gerekiyorsa diyagram eklenmiş mi?
- [ ] Gereksiz teknik detaylarla konu karmaşıklaştırılmış mı?
- [ ] Teknik olarak önemli detaylar atlanmış mı?
- [ ] İçerik doğrudan GitHub `.md` dosyasına yapıştırılabilir durumda mı?
- [ ] Gereksiz ChatGPT açıklamaları çıkarılmış mı?

---

# 21. Temel Amaç

Bu sözlüğün amacı kavramları ezberletmek değil, kavramlar arasındaki **teknik ilişkileri kurarak IT bilgisini bir bütün halinde geliştirmektir.**

Örneğin:

```text
SSD
 ↓
NAND Flash
 ↓
PCIe
 ↓
NVMe
 ↓
IOPS / Latency
 ↓
Storage Performance
 ↓
Server
 ↓
Virtualization
 ↓
Kubernetes
```

gibi ilişkilerin kurulabilmesi hedeflenmektedir.

Bu nedenle her yeni kavram mümkün olduğunca mevcut bilgi ağındaki diğer kavramlarla ilişkilendirilmelidir.

---

# 22. İçerik Üretim Kuralı

Yeni bir kavram istendiğinde:

1. Kavramın ait olduğu ana kategori belirlenir.
2. Gerekirse yeni alt kategori oluşturulur.
3. Kavramın ön koşulu olan temel bilgiler belirlenir.
4. Kavram en temel seviyeden açıklanır.
5. Çalışma mantığı anlatılır.
6. Benzer/alternatif teknolojiler karşılaştırılır.
7. Avantaj ve dezavantajlar açıklanır.
8. Kullanım alanları belirtilir.
9. Gerçek dünya örneği verilir.
10. İlgili diğer kavramlarla bağlantı kurulur.
11. Gerekiyorsa diyagram, tablo veya CLI örneği eklenir.
12. Son çıktı **doğrudan GitHub'a konulabilecek nihai Markdown içeriği** olarak hazırlanır.

ChatGPT'ye yönelik açıklamalar, taslak notlar veya "bu bölümü daha sonra doldur" gibi ifadeler nihai sözlük içeriğine dahil edilmez.
