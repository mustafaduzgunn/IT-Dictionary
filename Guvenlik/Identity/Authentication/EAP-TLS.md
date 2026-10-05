# EAP-TLS

## Tanım

**EAP-TLS (Extensible Authentication Protocol - Transport Layer Security)**, kullanıcı veya cihaz kimliğini **dijital sertifikalar kullanarak doğrulayan bir EAP authentication yöntemidir**.

EAP-TLS, TLS protokolünün sertifika tabanlı authentication ve güvenli anahtar üretme yeteneklerini EAP framework'ü içerisinde kullanır.

En önemli özelliği **mutual authentication (karşılıklı kimlik doğrulama)** desteklemesidir:

* Client, authentication server'a kendi sertifikasıyla kimliğini kanıtlar.
* Authentication server da kendi sertifikasıyla client'a kimliğini kanıtlar.

Bu nedenle EAP-TLS'te yalnızca kullanıcının veya cihazın kimliği doğrulanmaz; client aynı zamanda bağlandığı authentication server'ın da gerçekten güvenilir olduğunu doğrulayabilir.

---

## Neden Gereklidir?

Network erişiminde yalnızca username/password kullanmak, özellikle kurumsal ortamlarda bazı güvenlik problemleri oluşturabilir.

Örneğin:

```text
Username + Password
        ↓
Kimlik doğrulama
        ↓
Network erişimi
```

Password:

* çalınabilir,
* phishing ile ele geçirilebilir,
* tekrar kullanılabilir,
* kullanıcı tarafından paylaşılabilir,
* brute-force saldırılarına hedef olabilir.

EAP-TLS'te ise authentication için client tarafında bir **digital certificate** ve buna karşılık gelen **private key** bulunur.

```text
Client
 ├── Certificate
 └── Private Key
          ↓
      EAP-TLS
          ↓
Authentication Server
```

Bu yapı, özellikle cihaz kimliğinin güçlü biçimde doğrulanmasının önemli olduğu enterprise network ortamlarında kullanışlıdır.

---

## Hangi Problemi Çözer?

EAP-TLS temel olarak şu problemi çözer:

> "Network'e bağlanmaya çalışan cihazın gerçekten yetkili bir cihaz olduğunu nasıl güvenilir şekilde doğrularım?"

Username/password tabanlı authentication yerine certificate-based authentication kullanılmasını sağlar.

Ayrıca authentication sırasında server'ın da client tarafından doğrulanabilmesi sayesinde **rogue authentication server** riskinin azaltılmasına yardımcı olur.

---

## Nasıl Çalışır?

EAP-TLS genellikle **802.1X** gibi bir erişim kontrol mekanizması içerisinde görülür.

Basitleştirilmiş mimari:

```text
┌──────────────┐
│    Client    │
│   (EAP Peer) │
└──────┬───────┘
       │
       │ EAP
       │
┌──────▼───────┐
│ Authenticator│
│    Switch /  │
│      AP      │
└──────┬───────┘
       │
       │ EAP
       │
       │ RADIUS
       │
┌──────▼──────────────┐
│ Authentication      │
│ Server              │
│                     │
│ RADIUS / EAP Server │
└─────────────────────┘
```

Burada üç temel rol vardır:

* **EAP Peer:** Authentication isteyen client.
* **EAP Authenticator:** Client'ın network erişimini kontrol eden cihaz. Örneğin switch veya wireless AP.
* **EAP Server:** EAP authentication işlemini gerçekleştiren backend authentication server.

Authenticator çoğu durumda authentication işlemini kendisi gerçekleştirmek yerine EAP mesajlarını backend authentication server'a **pass-through** eder.

### Authentication Akışı

Basitleştirilmiş EAP-TLS akışı şu şekildedir:

```text
Client                  Authenticator             EAP Server
  │                           │                       │
  │──── EAP-Start ───────────>│                       │
  │                           │                       │
  │<── EAP-Request/Identity ──│                       │
  │                           │                       │
  │──── EAP-Response/Identity ──────────────────────>│
  │                           │                       │
  │<──────────── EAP-TLS/Start ──────────────────────│
  │                           │                       │
  │──── TLS ClientHello ────────────────────────────>│
  │                           │                       │
  │<── TLS ServerHello + Server Certificate ─────────│
  │                           │                       │
  │──── Client Certificate ─────────────────────────>│
  │                           │                       │
  │──── Certificate Verify ─────────────────────────>│
  │                           │                       │
  │<──────────── TLS Finished ───────────────────────│
  │                           │                       │
  │<──────────── EAP-Success ────────────────────────│
  │                           │                       │
  │──── Network Access ──────>│                       │
```

Gerçek protokol mesajları ve TLS handshake, EAP paketlerinin içerisinde taşınır.

---

## Sertifika Doğrulaması

EAP-TLS'in güvenlik modelinin temelinde **PKI (Public Key Infrastructure)** bulunur.

Tipik yapı:

```text
                 Root CA
                   │
                   │
              Intermediate CA
                /        \
               /          \
              ↓            ↓
       Client Certificate   Server Certificate
              │            │
              ↓            ↓
           Client       Authentication
                          Server
```

Client'ın certificate'ı bir güvenilir **Certificate Authority (CA)** tarafından imzalanır.

Authentication server da kendi certificate'ına sahiptir.

Client authentication server'ın certificate'ını doğrular.

Server ise client certificate'ını doğrular.

Bu nedenle EAP-TLS **certificate-based mutual authentication** sağlar.

---

## Temel Bileşenler

### EAP

**EAP (Extensible Authentication Protocol)** farklı authentication yöntemlerinin kullanılmasını sağlayan bir framework'tür.

EAP-TLS, bu framework içerisindeki authentication yöntemlerinden biridir.

### TLS

**TLS (Transport Layer Security)** güvenli iletişim ve authentication mekanizmalarını sağlar.

EAP-TLS, TLS handshake'i EAP mesajları içerisinde taşır.

### Digital Certificate

Client ve server kimliklerinin doğrulanmasında kullanılır.

### Private Key

Certificate'ın sahibi olduğunu kanıtlamak için kullanılır.

Private key client üzerinde güvenli şekilde saklanmalıdır.

### Certificate Authority

Certificate'ların güvenilir bir CA tarafından imzalanması, tarafların birbirlerinin kimliğini doğrulayabilmesini sağlar.

### Authentication Server

EAP-TLS authentication işleminin backend tarafındaki endpoint'idir.

Enterprise ortamlarda genellikle RADIUS tabanlı bir authentication altyapısıyla birlikte kullanılır.

---

## EAP-TLS ve Username/Password Authentication

| Özellik                    | Username/Password              | EAP-TLS     |
| -------------------------- | ------------------------------ | ----------- |
| Authentication             | Kullanıcı bilgisi              | Certificate |
| Private key                | Yok                            | Var         |
| Phishing riski             | Daha yüksek                    | Daha düşük  |
| Mutual authentication      | Authentication yöntemine bağlı | Desteklenir |
| PKI gereksinimi            | Genellikle yok                 | Evet        |
| Certificate yönetimi       | Yok                            | Gerekir     |
| Operasyonel karmaşıklık    | Düşük                          | Daha yüksek |
| Cihaz bazlı authentication | Sınırlı                        | Güçlü       |

EAP-TLS'in en önemli avantajı, authentication'ın yalnızca kullanıcı tarafından bilinen bir password'e bağlı olmamasıdır.

---

## EAP-TLS ve PEAP

EAP-TLS ile **PEAP (Protected Extensible Authentication Protocol)** birbirinden farklı authentication yöntemleridir.

| Özellik               | EAP-TLS                   | PEAP                                      |
| --------------------- | ------------------------- | ----------------------------------------- |
| TLS kullanımı         | Evet                      | Evet                                      |
| Client certificate    | Gereklidir                | Genellikle gerekmez                       |
| Server certificate    | Kullanılır                | Kullanılır                                |
| Client authentication | Certificate               | TLS tunnel içerisindeki başka EAP yöntemi |
| PKI gereksinimi       | Client + Server tarafında | Esas olarak Server tarafında              |
| Yönetim karmaşıklığı  | Daha yüksek               | Daha düşük                                |
| Güvenlik modeli       | Certificate-based         | Protected tunnel + inner authentication   |

Örneğin PEAP içerisinde username/password tabanlı bir inner authentication yöntemi kullanılabilirken EAP-TLS doğrudan client certificate'ı authentication'ın temel unsuru olarak kullanır.

---

## Avantajları

* Güçlü certificate-based authentication sağlar.
* Mutual authentication destekler.
* Password kullanımına olan bağımlılığı azaltır.
* Client ve server'ın birbirlerini doğrulamasına olanak sağlar.
* Enterprise networklerde cihaz kimliğinin doğrulanması için uygundur.
* 802.1X wired ve wireless networklerde kullanılabilir.
* Authentication sonrasında kullanılacak keying material'ın üretilmesini sağlar.

---

## Dezavantajları

* PKI altyapısı gerektirir.
* Client certificate'larının dağıtılması gerekir.
* Certificate lifecycle yönetimi gerekir.
* Certificate expiration durumları operasyonel sorun oluşturabilir.
* Certificate revocation mekanizmasının yönetilmesi gerekir.
* Kullanıcı veya cihaz sayısı arttıkça certificate management karmaşıklaşabilir.
* İlk kurulum ve troubleshooting, username/password tabanlı authentication'a göre daha karmaşıktır.

---

## Nerelerde Kullanılır?

EAP-TLS özellikle kurumsal network authentication senaryolarında kullanılır.

### Wired Network

802.1X destekli switch portlarında:

```text
Laptop
   │
   │ 802.1X / EAP-TLS
   │
Switch
   │
   │ RADIUS
   │
Authentication Server
```

Client'ın network erişimi authentication sonucuna göre açılabilir.

### Wireless Network

Enterprise Wi-Fi authentication'da kullanılabilir:

```text
Client
   │
   │ EAP-TLS
   ↓
Wireless AP
   │
   │ RADIUS
   ↓
Authentication Server
```

### Network Access Control

EAP-TLS, cihaz kimliğinin güçlü şekilde doğrulanmasının istendiği NAC mimarilerinin authentication katmanında kullanılabilir.

---

## Gerçek Hayattan Örnek

Bir kurumda çalışan laptopların kurumsal Wi-Fi ağına yalnızca kurum tarafından yönetilen cihazların bağlanmasına izin verildiğini düşünelim.

Her kurumsal laptop'a kurumun PKI altyapısı üzerinden bir client certificate yüklenir.

```text
Corporate Laptop
       │
       │ Client Certificate
       ↓
   Enterprise Wi-Fi
       │
       │ RADIUS
       ↓
 Authentication Server
       │
       ↓
 Certificate Validation
       │
       ├── Valid → Access
       │
       └── Invalid → Reject
```

Çalışanın username/password bilgilerini bilmesi tek başına yeterli olmayabilir. Cihazın geçerli bir certificate'a ve corresponding private key'e sahip olması gerekir.

Bu yaklaşım özellikle **enterprise Wi-Fi, 802.1X ve NAC** mimarilerinde anlamlıdır.

---

## Ne Zaman Tercih Edilir?

```text
Güçlü cihaz kimliği gerekiyorsa
        ↓
Certificate-based authentication
        ↓
EAP-TLS tercih edilebilir.
```

Özellikle:

* Kurumsal Wi-Fi authentication
* 802.1X wired authentication
* NAC
* Managed endpoint authentication
* Yüksek güvenlik gerektiren enterprise networkler

için uygundur.

Buna karşılık PKI altyapısının bulunmadığı veya certificate lifecycle yönetiminin operasyonel olarak mümkün olmadığı küçük ortamlarda daha basit EAP yöntemleri tercih edilebilir.

---

## Diğer Konularla İlişkisi

```text
EAP-TLS
├── EAP
├── TLS
├── 802.1X
├── RADIUS
├── PKI
│   ├── CA
│   ├── Certificate
│   └── Certificate Revocation
├── Digital Identity
├── NAC
├── Enterprise Wi-Fi
└── Network Access Control
```

EAP-TLS'in anlaşılması için özellikle **EAP, TLS, 802.1X, RADIUS ve PKI** kavramlarının birlikte değerlendirilmesi gerekir.

---

## İleri Seviye: Key Derivation

EAP-TLS yalnızca authentication gerçekleştirmez; authentication sonucunda network access için kullanılabilecek **keying material** da üretir.

EAP-TLS içerisinde authentication sonucunda **MSK (Master Session Key)** ve **EMSK (Extended Master Session Key)** gibi keying material türetilir.

Bu yapı, EAP'in üzerinde çalıştığı lower layer tarafından network erişiminin güvenli şekilde kurulmasında kullanılabilir.

---

## Güncel TLS Sürümleri

EAP-TLS'in ilk standart tanımı RFC 5216 ile yapılmıştır. Günümüzde EAP-TLS'in TLS 1.3 ile kullanımı ayrıca **EAP-TLS 1.3** olarak tanımlanmıştır.

Bu nedenle modern sistemlerde EAP-TLS konuşulurken yalnızca klasik TLS 1.0/1.1 davranışı değil, kullanılan implementasyonun desteklediği TLS sürümü de dikkate alınmalıdır.

---

## Bilinmesi Gereken İlgili Kavramlar

* [EAP](./eap.md)
* [TLS](./tls.md)
* [802.1X](./802-1x.md)
* [RADIUS](./radius.md)
* [PKI](./pki.md)
* [Certificate](./certificate.md)
* [Certificate Authority](./certificate-authority.md)
* [PEAP](./peap.md)
* [EAP-TTLS](./eap-ttls.md)
* [NAC](../network-security/nac.md)
* [Enterprise Wi-Fi](../../network/wireless/enterprise-wifi.md)
