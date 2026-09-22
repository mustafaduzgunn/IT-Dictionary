# DNS

## Tanım

**Domain Name System (DNS)**, insanlar tarafından kolay hatırlanan alan adlarını (`example.com`) bilgisayarların ve network cihazlarının kullandığı IP adresleriyle (`93.184.216.34`) ilişkilendiren dağıtık ve hiyerarşik bir isim çözümleme sistemidir.

En basit haliyle DNS:

```text
Alan adı
   ↓
example.com
   ↓
DNS
   ↓
93.184.216.34
```

Ancak DNS yalnızca "domain → IP" dönüşümü yapan basit bir sistem değildir. Mail sunucularının bulunması, bir domain için hangi DNS sunucularının yetkili olduğunun belirlenmesi, farklı servislerin tanımlanması ve domain hiyerarşisinin yönetilmesi gibi birçok görevi vardır.

---

## Neden Gereklidir?

Network üzerindeki sistemler birbirleriyle IP adresleri üzerinden iletişim kurabilir:

```text
Client → 93.184.216.34
```

Ancak insanların IP adreslerini ezberlemesi ve kullanması pratik değildir.

DNS sayesinde:

```text
Client
  |
  | "www.example.com hangi IP'de?"
  ↓
DNS
  |
  | "93.184.216.34"
  ↓
Client
  |
  ↓
93.184.216.34
```

şeklinde isimleri kullanarak servislere erişilebilir.

DNS olmasaydı kullanıcıların web sitelerine ve diğer servislere erişebilmek için IP adreslerini bilmeleri gerekirdi.

---

## Hangi Problemi Çözer?

DNS temel olarak **isim çözümleme (name resolution)** problemini çözer.

Örneğin bir kullanıcı:

```text
www.example.com
```

adresine erişmek istediğinde işletim sistemi bu ismi DNS üzerinden sorgulayarak ilgili kaydı bulabilir.

Sonuç:

```text
www.example.com → 93.184.216.34
```

olabilir.

DNS yalnızca IPv4 adreslerini değil, farklı bilgi türlerini de taşıyabilir:

```text
A       → IPv4 adresi
AAAA    → IPv6 adresi
CNAME   → Başka bir DNS adı
MX      → Mail sunucusu
NS      → Yetkili DNS sunucusu
TXT     → Metinsel bilgi
SOA     → Zone'un temel yetki bilgileri
```

---

## DNS Hiyerarşisi

DNS merkezi bir veritabanı değildir.

Hiyerarşik ve dağıtık bir yapı kullanır.

Basitleştirilmiş DNS hiyerarşisi:

```text
                         .
                         │
                  Root DNS Servers
                         │
          ┌──────────────┼──────────────┐
          │              │              │
         .com           .tr            .org
          │              │
     TLD DNS         TLD DNS
          │
     example.com
          │
    Authoritative DNS
          │
   ┌──────┼─────────┐
   │      │         │
   www    mail      ns
```

Buradaki önemli seviyeler:

```text
Root
  ↓
TLD
  ↓
Authoritative DNS
  ↓
DNS Records
```

---

## Root DNS

DNS hiyerarşisinin en üst seviyesinde **Root** bulunur.

Root zone:

```text
.
```

şeklinde gösterilir.

Örneğin bir DNS resolver:

```text
www.example.com
```

için doğrudan cevabı bilmiyorsa DNS hiyerarşisini takip edebilir.

Basitleştirilmiş akış:

```text
Resolver
   |
   | www.example.com?
   ↓
Root
   |
   | .com DNS sunucuları
   ↓
.com TLD
   |
   | example.com authoritative DNS
   ↓
Authoritative DNS
   |
   | www.example.com = 93.184.216.34
   ↓
Resolver
```

Root sunucuları genellikle `www.example.com` için doğrudan IP adresini vermez.

Bunun yerine `.com` gibi ilgili **Top-Level Domain (TLD)** DNS sunucularına yönlendirme sağlar.

---

## TLD DNS

**Top-Level Domain (TLD)**, domain adının en üst seviyedeki uzantısını yönetir.

Örneğin:

```text
example.com
       ↑
      TLD
```

Burada:

```text
.com
```

TLD'dir.

Başka örnekler:

```text
.tr
.org
.net
.edu
```

Bir resolver `example.com` için Root DNS'e sorduğunda Root, `.com` TLD DNS sunucularına ulaşabilmesi için gerekli bilgiyi sağlar.

Daha sonra `.com` TLD DNS sunucuları `example.com` domaininin **authoritative DNS sunucularını** gösterir.

---

## Authoritative DNS

**Authoritative DNS Server**, belirli bir domain için DNS kayıtlarının otoritesine sahip olan DNS sunucusudur.

Örneğin:

```text
example.com
```

domaininin authoritative DNS sunucusu:

```text
ns1.example-dns.com
ns2.example-dns.com
```

olabilir.

Bu sunucularda örneğin:

```text
www.example.com → 93.184.216.34
mail.example.com → 93.184.216.50
```

gibi kayıtlar bulunabilir.

Önemli nokta:

> Authoritative DNS, kendi sorumlu olduğu zone hakkında kesin DNS bilgisini sağlar.

---

## DNS Zone

**DNS Zone**, belirli bir DNS namespace bölümünün yönetildiği alandır.

Örneğin:

```text
example.com
```

için bir DNS zone bulunabilir.

Bu zone içerisinde:

```text
www.example.com
mail.example.com
dns1.example.com
```

gibi kayıtlar bulunabilir.

Basitleştirilmiş olarak:

```text
example.com zone
│
├── SOA
├── NS
├── www    → A
├── mail   → A
└── @      → MX
```

Domain ile zone aynı kavram değildir.

Bir domain:

```text
example.com
```

olabilirken DNS yönetimi delegation kullanılarak alt zone'lara ayrılabilir.

Örneğin:

```text
example.com
│
├── www.example.com
├── mail.example.com
│
└── sub.example.com
      │
      └── ayrı DNS zone
```

şeklinde bir yapı kurulabilir.

---

## DNS Resolver

Kullanıcının bilgisayarında çalışan uygulama genellikle DNS hiyerarşisini doğrudan takip etmez.

Bunun yerine bir **DNS Resolver** kullanır.

Örneğin:

```text
Client
   |
   | www.example.com
   ↓
Recursive DNS Resolver
   |
   ↓
Root
   ↓
.com
   ↓
Authoritative DNS
   ↓
93.184.216.34
```

Resolver'ın görevi DNS sorgusunu çözmek ve sonucu client'a döndürmektir.

Resolver:

* Cache kullanabilir.
* Root DNS'e sorgu gönderebilir.
* TLD DNS'e sorgu gönderebilir.
* Authoritative DNS'e sorgu gönderebilir.
* Sonucu client'a döndürebilir.

---

## Recursive Query ve Iterative Query

DNS sorgularında önemli iki kavram vardır.

### Recursive Query

Client resolver'a:

> "Bu domainin cevabını benim için bul."

der.

Örneğin:

```text
Client
   |
   | www.example.com?
   ↓
Resolver
```

Resolver gerekli diğer DNS sunucularına kendisi ulaşarak sonucu bulur.

Client açısından:

```text
Client → Resolver
```

şeklinde tek bir DNS sorgusu gibi görünür.

---

### Iterative Query

Resolver, DNS hiyerarşisindeki sunuculara sorgu yaptığında sunucu doğrudan nihai cevabı vermek yerine başka bir DNS sunucusunu gösterebilir.

Örneğin:

```text
Resolver → Root

Root:
".com TLD sunucularına sor."

Resolver → .com TLD

TLD:
"example.com authoritative DNS sunucularına sor."

Resolver → Authoritative DNS

Authoritative:
"www.example.com = 93.184.216.34"
```

Bu yapı DNS'in dağıtık çalışmasının temel parçalarından biridir.

---

## DNS Record

DNS bilgileri **resource record** adı verilen kayıtlarla tutulur.

En önemli kayıt türleri şunlardır:

| Record | Görevi                                              |
| ------ | --------------------------------------------------- |
| A      | Hostname → IPv4                                     |
| AAAA   | Hostname → IPv6                                     |
| CNAME  | Bir DNS adını başka bir DNS adına yönlendirir       |
| NS     | Bir zone'un authoritative DNS sunucularını belirtir |
| MX     | Mail sunucularını belirtir                          |
| TXT    | Metinsel bilgi taşır                                |
| SOA    | Zone'un temel otorite ve yönetim bilgilerini içerir |
| PTR    | IP adresinden hostname'e çözümleme sağlar           |

---

## A Record

**A (Address) Record**, bir hostname'i IPv4 adresine eşler.

Örneğin:

```text
www.example.com → 93.184.216.34
```

DNS zone içerisinde kabaca:

```text
www    IN    A    93.184.216.34
```

şeklinde bulunabilir.

---

## AAAA Record

**AAAA Record**, hostname'i IPv6 adresine eşler.

Örneğin:

```text
www.example.com → 2001:db8::10
```

```text
www    IN    AAAA    2001:db8::10
```

---

## CNAME Record

**Canonical Name (CNAME)**, bir DNS adını başka bir DNS adına yönlendirir.

Örneğin:

```text
www.example.com
        ↓
web.example.com
        ↓
93.184.216.34
```

Burada:

```text
www.example.com → web.example.com
```

CNAME ile tanımlanabilir.

CNAME doğrudan IP adresi taşımaz; başka bir DNS adına işaret eder.

---

## MX Record

**Mail Exchange (MX)** kaydı, bir domain için mail sunucularını belirtir.

Örneğin:

```text
example.com
    |
    └── MX → mail.example.com
```

Bir kullanıcı:

```text
user@example.com
```

adresine mail gönderdiğinde gönderen mail sistemi `example.com` domaininin MX kayıtlarını sorgulayabilir.

---

## NS Record

**Name Server (NS)** kaydı, bir DNS zone için authoritative DNS sunucularını belirtir.

Örneğin:

```text
example.com
    |
    ├── NS → ns1.example-dns.com
    └── NS → ns2.example-dns.com
```

NS kayıtları DNS delegation mekanizmasının önemli parçalarındandır.

---

## Parent ve Child DNS

DNS hiyerarşisini anlamak için **parent** ve **child** ilişkisini anlamak önemlidir.

Örneğin:

```text
example.com
     │
     └── sub.example.com
```

Burada:

```text
example.com
```

parent,

```text
sub.example.com
```

child namespace olarak düşünülebilir.

Eğer `sub.example.com` ayrı bir DNS zone olarak yönetilecekse parent zone, child zone'un authoritative DNS sunucularına **delegation** yapabilir.

Örneğin:

```text
example.com zone
       |
       | delegation
       ↓
sub.example.com
       |
       ↓
ns1.sub.example.com
```

Bu sayede `sub.example.com` altındaki DNS kayıtlarının yönetimi farklı DNS sunucularına bırakılabilir.

---

## DNS Delegation

**DNS Delegation**, bir DNS namespace'in yönetiminin başka authoritative DNS sunucularına devredilmesidir.

Örneğin:

```text
example.com
│
├── www.example.com
├── mail.example.com
│
└── sub.example.com
        │
        └── ayrı authoritative DNS
```

Parent zone:

```text
example.com
```

`sub.example.com` için:

```text
NS
```

kayıtları üzerinden child zone'un authoritative DNS sunucularını gösterebilir.

Bu yapı DNS'in büyük ve yönetilebilir bir hiyerarşi halinde çalışmasını sağlar.

---

## DNS Sorgusunun Gerçekleşmesi

Bir kullanıcı browser'a:

```text
www.example.com
```

yazdığında basitleştirilmiş süreç şöyledir:

```text
1. Kullanıcı
      |
      | www.example.com
      ↓
2. İşletim sistemi
      |
      | DNS query
      ↓
3. Recursive Resolver
      |
      ↓
4. Root DNS
      |
      ↓
5. .com TLD DNS
      |
      ↓
6. example.com Authoritative DNS
      |
      ↓
7. A Record
      |
      ↓
93.184.216.34
      |
      ↓
8. Resolver
      |
      ↓
9. Client
      |
      ↓
10. Web Server
```

Gerçek sistemlerde cache nedeniyle her sorguda bu adımların tamamı gerçekleşmez.

---

## DNS Cache

DNS sorgularının her seferinde Root → TLD → Authoritative DNS zincirini takip etmesi gereksiz yük oluşturur.

Bu nedenle DNS sonuçları cache'lenir.

Örneğin resolver:

```text
www.example.com
        ↓
93.184.216.34
```

sonucunu belirli bir süre cache'de tutabilir.

Bu süre **TTL (Time To Live)** ile belirlenir.

Örneğin:

```text
www.example.com
TTL = 300
A = 93.184.216.34
```

TTL 300 saniye ise resolver bu kaydı belirli koşullar altında 300 saniyeye kadar cache'den kullanabilir.

---

## DNS ve Port

DNS genellikle:

```text
UDP/53
```

üzerinden çalışır.

Bazı durumlarda:

```text
TCP/53
```

de kullanılır.

Örneğin DNS response'unun UDP üzerinden taşınamayacak kadar büyük olması veya **zone transfer** gibi işlemler TCP kullanımını gerektirebilir.

Modern DNS teknolojilerinde DNS trafiğinin farklı taşıma yöntemleri de bulunur:

```text
DNS over TLS (DoT)
DNS over HTTPS (DoH)
```

Bunlar klasik DNS sorgusunun güvenli/şifreli taşıma yöntemleridir.

---

## Reverse DNS

Normal DNS çözümlemesinde:

```text
Hostname → IP
```

yapılır.

Örneğin:

```text
www.example.com
        ↓
93.184.216.34
```

Reverse DNS ise:

```text
IP → Hostname
```

çözümlemesidir.

Bunun için genellikle:

```text
PTR Record
```

kullanılır.

Örneğin:

```text
93.184.216.34
        ↓
www.example.com
```

---

## DNS'in Diğer Teknolojilerle İlişkisi

DNS tek başına çalışan bir teknoloji değildir.

Örneğin bir web erişiminde:

```text
User
  |
  ↓
DNS
  |
  ↓
IP Address
  |
  ↓
Routing
  |
  ↓
TCP
  |
  ↓
TLS
  |
  ↓
HTTP/HTTPS
  |
  ↓
Web Server
```

Benzer şekilde kurumsal bir ortamda:

```text
Client
  |
  ├── DHCP → IP configuration
  |
  └── DNS → Name resolution
               |
               └── Application / Server
```

DNS ayrıca:

* Active Directory
* DHCP
* Web
* Email
* Load Balancing
* Cloud
* Kubernetes
* Network Management
* Security

gibi birçok teknolojiyle doğrudan ilişkilidir.

---

## DNS ve DHCP İlişkisi

DHCP bir client'a network yapılandırmasını verirken DNS sunucusunun adresini de sağlayabilir.

Örneğin:

```text
Client
  |
  | DHCP
  ↓
IP Address: 10.10.10.25
Subnet:     255.255.255.0
Gateway:    10.10.10.1
DNS:        10.10.10.10
```

Client daha sonra:

```text
server.example.local
```

gibi bir isim için:

```text
10.10.10.10
```

adresindeki DNS resolver'a sorgu gönderebilir.

---

## Gerçek Hayattan Örnek

Bir enterprise ortamını düşünelim:

```text
Client
10.10.20.50
    |
    | DNS Query
    ↓
DNS Resolver
10.10.10.10
    |
    ↓
Internal DNS Zone
example.local
    |
    ├── dc01.example.local
    ├── dns01.example.local
    ├── mail.example.local
    └── app01.example.local
```

Kullanıcı:

```text
https://app01.example.local
```

adresine eriştiğinde browser'ın uygulama sunucusunun IP adresini öğrenebilmesi için DNS çözümlemesi yapılır.

Örneğin:

```text
app01.example.local
        ↓
10.10.30.100
```

Sonrasında client bu IP adresine network üzerinden erişir.

---

## Public DNS ve Internal DNS

DNS altyapıları kullanım alanına göre farklı olabilir.

### Public DNS

Internet üzerinden erişilebilen domainlerin DNS kayıtlarını yönetir.

Örneğin:

```text
example.com
```

için:

```text
www.example.com
mail.example.com
```

gibi public kayıtlar bulunabilir.

### Internal DNS

Kuruluşun kendi networkünde kullanılan DNS altyapısıdır.

Örneğin:

```text
server01.corp.example.local
printer01.corp.example.local
dc01.corp.example.local
```

gibi isimler yalnızca internal DNS üzerinden çözülebilir.

---

## DNS Avantajları

* İnsanların IP adresleri yerine isim kullanmasını sağlar.
* Dağıtık ve hiyerarşik bir yapı sunar.
* Büyük networklerin yönetilebilir olmasını sağlar.
* Cache sayesinde sorgu trafiğini azaltır.
* Farklı servis türleri için farklı record tipleri sunar.
* Delegation ile DNS yönetiminin farklı ekip veya sistemlere dağıtılmasını sağlar.
* Public ve internal ortamlar için kullanılabilir.

---

## DNS Sınırlamaları ve Dikkat Edilmesi Gerekenler

* Yanlış DNS kaydı servis erişimini doğrudan etkileyebilir.
* Cache nedeniyle DNS değişiklikleri anında tüm clientlara yansımayabilir.
* Authoritative DNS altyapısının erişilebilirliği kritik olabilir.
* DNS güvenliği ayrıca ele alınmalıdır.
* Yanlış yapılandırılmış DNS delegation çözümleme problemlerine neden olabilir.
* DNS yalnızca isim çözümleme sağlar; IP routing veya uygulama erişilebilirliğini tek başına garanti etmez.

---

## İleri Seviye DNS Kavramları

DNS temeli öğrenildikten sonra aşağıdaki konular incelenebilir:

* DNS Zone
* Zone Transfer
* AXFR
* IXFR
* SOA
* Glue Record
* DNS Delegation
* Recursive DNS
* Authoritative DNS
* DNS Cache
* TTL
* Split-Horizon DNS
* DNSSEC
* DoH
* DoT
* Anycast DNS
* Dynamic DNS
* Reverse DNS
* PTR
* SRV
* CAA
* DNS Load Balancing

---

## Diğer Konularla İlişkisi

```text
DNS
│
├── Network
│   ├── IP
│   ├── UDP
│   └── TCP
│
├── Internet
│   ├── Root DNS
│   ├── TLD
│   └── Domain
│
├── Services
│   ├── Web
│   ├── Mail
│   └── Application
│
├── Infrastructure
│   ├── DHCP
│   ├── Active Directory
│   └── Load Balancer
│
└── Security
    ├── DNSSEC
    ├── DoH
    ├── DoT
    └── DNS Filtering
```

---

## Bilinmesi Gereken İlgili Kavramlar

* Domain Name System (DNS)
* Domain
* Domain Name
* Root DNS
* Top-Level Domain (TLD)
* Authoritative DNS
* Recursive DNS Resolver
* DNS Zone
* DNS Delegation
* Parent Zone
* Child Zone
* A Record
* AAAA Record
* CNAME Record
* NS Record
* MX Record
* TXT Record
* SOA Record
* PTR Record
* DNS Cache
* TTL
* Reverse DNS
* DNSSEC
* DNS over HTTPS (DoH)
* DNS over TLS (DoT)
* Glue Record
* Zone Transfer
* AXFR
* IXFR
